# Lab 4 bis - Flow Modeler

## Overview
We will Build a predictive analytic model to determine whether a person has chronic kidney disease by using IBM Watson Machine Learning, and the Flow Editor with Spark MLlib nodes

You will use classification transformers with publicly available data about metabolic diseases from the University of California, Irvine to determine whether someone has chronic kidney disease or not.

Prerequisites: Ensure that you have at least one Spark service instance available and at least one project.

## Preparation
This tutorial machine learning model in IBM Watson Studio contains steps to get data fom an external source by using the Watson Studio flow creation tool: the Flow Editor. You can then explore the data before you deploy it.

Add research data to your Watson Studio project:   
The original source for this data is from the University of California, Irvine (UCI) and is available at the UCI Machine Learning Repository at https://archive.ics.uci.edu/ml/datasets/Chronic_Kidney_Disease

## Background:
The data set is the result of an extensive study based on hospital admissions over a period of time. Because of the types of tests that were performed as part of hospital admissions, the following items are available as part of this analysis:

* __serum creatinine (sc)__: A serum creatinine test measures the level of creatinine in your blood and provides an estimate of how well your kidneys filter.
* __age (age)__: The age of the test subject.
* __diabetes mellitus (dm)__: A group of metabolic diseases in which there are high blood sugar levels over a prolonged period

These items are strikingly important when predicting chronic kidney disease. Some other fields are relatively important (e.g., blood pressure), but were omitted from the features list arbitrarily.

1. Download the `chronic_kidney_disease_full.csv` file.
Add the file to the data assets for the project. Click add data assets, click browse, select the data file, click Open, and then click Apply.
1. Create a machine learning flow.
From the project assets tab page, navigate to `Modeler Flows` and click `New Flow` ![](images_4bis/markdown-img-paste-20180515232202258.png)
1. Type a name and description for the machine learning flow, select `Modeler Flow`, and `IBM SPSS Modeler` for the runtime, and `[Create]` ![](images_4bis/markdown-img-paste-20180515232544495.png)
1. The Flow Editor should open. Drag&Drop the `Data Asset` icon from the `Import` drawer onto the canvas: ![](images_4bis/markdown-img-paste-20180515233055832.png)
1. double-click the data asset node and change the data asset ![](images_4bis/markdown-img-paste-20180515233155905.png)
1. Select the `chronic_kidney_disease_full.csv` file. Don't forget to `[Save]` with the button at the bottom right

## Transform and train the data
After you load the data, you must transform the data by using transformers. You will be creating a simple machine learning flow by dragging transformers and estimators onto the Flow Editor and connecting them to the data source.

1. Open the palette by clicking the palette node ![](images_4bis/markdown-img-paste-20180515233613436.png).
1. Add the following nodes from the palette:
    1. Filter rows: click the `Record Operations` tab and drag the `Select` node to the Flow Editor![](images_4bis/markdown-img-paste-20180516001950760.png)
    2. Wire the node to the data asset node, by hovering near the data asset node, clicking the highlighted area and dragging a connector to the Select node. ![](images_4bis/markdown-img-paste-20180516002043660.png)
    3. Configure the Select node to limit the data to the high-risk group of people over the age of 40 years. Double-click the node to open its properties and enter `Mode` `Discard` and `Condition` `age<40` ![](images_4bis/markdown-img-paste-20180516002438253.png)
    1. Remove columns to clean up the data, we will remove some of the columns that are not necessary to the analysis or that have missing data points. For this, you use the `Filter` node from the `Field Operations` drawer, drop to canvas and wire to the previous `Select` node: ![](images_4bis/markdown-img-paste-20180516002731727.png)
    1. Double-click the Filter node, expand the Settings section, and click Add Columns. Because the following fields have many missing values, remove them by selecting them in the list: sg, al, su, rbc, pc, pcc, ba, bgr, bu, sod, pot, hemo, pcv, wbcc, rbcc, htn, cad, appet, pe, and ane, and then click OK.![](images_4bis/markdown-img-paste-20180516003103850.png)
    1. make sure the Mode section is set to the filter the selected fields and click `[Save]` ![](images_4bis/markdown-img-paste-20180516003139877.png)
    1.When you finish this portion of the tutorial, your machine learning flow should look like the following image: ![](images_4bis/markdown-img-paste-20180516003223845.png)
    1. Set target for decision: Add a `Type` node from `Field Operations` drawer, connect to Filter node: ![](images_4bis/markdown-img-paste-20180516003928229.png)
    1. Add a classification algorithm. 
      1. Double-click to open properties, and Click on configure types. Add column name `class`, change role to target hit ok ![](images_4bis/markdown-img-paste-20180516004156623.png) and then save:
      1. One more step before building the classification model is to divide data into train and test sets. We will use the Partition node for this. Drop it on the canvas, wire to Type node, open properties to set partition to 70/30: ![](images_4bis/markdown-img-paste-20180516004453551.png)
      1. Finally let’s fit the classification model. We will be using a C5.0 algorithm to build a decision tree . A C5.0 model works by splitting the data based on the field that provides the maximum information gain.You can see node `C5.0` under the `Modeling` section of the nodes palette. ![](images_4bis/markdown-img-paste-2018051600472954.png)
  1. Run and deploy the model. After you create the machine learning flow, you must run and then deploy it. ![](images_4bis/markdown-img-paste-20180516005011700.png)
  1. Once you run your decision tree model you will be able to see your model in a golden color node. ![](images_4bis/markdown-img-paste-20180516005052265.png)
  1. Right-click on the golden color node and view the model. You can see predictor importance, tree diagram, and other model information here.
  1. To evaluate the performance of the model, select the Analysis node from the Output section of the node palette and connect it with the model. Similarly, use the Table node to view data in a table format with predicted labels and confidence: ![](images_4bis/markdown-img-paste-20180516005417729.png)
  1. Open the Analysis view result, here we have achieved 95% accuracy on our test data set with this model ![](images_4bis/markdown-img-paste-20180516010227873.png)

What does this tell us? While this example is basic in nature, it provides insight into the fact that older people tend to have a higher probability of getting chronic kidney disease than younger people, controlling for other factors. It also shows the importance of serum creatinine in diagnosing kidney disease.

