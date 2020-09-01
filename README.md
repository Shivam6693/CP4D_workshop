# Predict Fraud Auto Insurance claim using Watson Machine Learning and Jupyter Notebooks on Cloud Pak for Data

In this Code Pattern, we use IBM Cloud Pak for Data to go through the whole data science pipeline to solve a business problem and predict customer churn using an Auto Insurance claims dataset. Cloud Pak for Data is an interactive, collaborative, cloud-based environment where data scientists, developers, and others interested in data science can use tools (e.g., RStudio, Jupyter Notebooks, Spark, etc.) to collaborate, share, and gather insight from their data as well as build and deploy machine learning and deep learning models.

When the reader has completed this Code Pattern, they will understand how to:

* Use [Jupyter Notebooks](https://jupyter.org/) to load, visualize, and analyze data
* Run Notebooks in [IBM Cloud Pak for Data](https://www.ibm.com/analytics/cloud-pak-for-data)
* Build, test, and deploy a machine learning model using [Scikit-learn](https://scikit-learn.org/stable/) on IBM Cloud Pak for Data.
* Deploy a selected machine learning model to production using Cloud Pak for Data
* Create a front-end application to interface with the client and start consuming your deployed model.
* Test the deployed model using the exposed endpoint


## Flow

1. User loads the Jupyter notebook into the Cloud Pak for Data platform.
2.	Fraud claims dataset is loaded into the Jupyter Notebook as virtualized data after following the Data virtualization and Data refinery tutorial.
3. Preprocess the data, build machine learning models and save to Watson Machine Learning on Cloud Pak for Data.
4. Deploy a selected machine learning model into production on the Cloud Pak for Data platform and obtain a scoring endpoint.
5. Test the deployed model using the scoring endpoint.

## Included components

* [IBM Cloud Pak for Data](https://www.ibm.com/products/cloud-pak-for-data)
* [Watson Machine Learning Add On for Cloud Pak for Data](https://www.ibm.com/cloud/machine-learning)

## Featured technologies

* [Jupyter Notebooks](https://jupyter.org/): An open-source web application that allows you to create and share documents that contain live code, equations, visualizations, and explanatory text.
* [Pandas](https://pandas.pydata.org/):  An open source library providing high-performance, easy-to-use data structures and data analysis tools for the Python programming language.
* [Scikit-learn](https://scikit-learn.org/stable/): An open source machine learning library in Python

## Prerequisites

* [IBM Cloud Pak for Data](https://www.ibm.com/analytics/cloud-pak-for-data)
* [Watson Machine Learning Add On for Cloud Pak for Data](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/wsj/analyze-data/ml-install-overview.html)

## Steps



1. [Import notebook to Cloud Pak for Data](#1-import-notebook-to-cloud-pak-for-data)
1. [Run the notebook](#2-run-the-notebook)
1. [Create a Space for Machine Learning Deployments](#3-create-a-space-for-machine-learning-deployments)
1. [Deploying the model](#4-deploying-the-model-using-the-Cloud-Pak-for-Data-UI)
1. [Testing the model](#5-testing-the-model)



### 1. Import notebook to Cloud Pak for Data

* In your project, either click the `Add to project +` button, and choose `Notebook`

![Add notebook](doc/source/images/wml-1-add-asset.png)

* On the next screen, select the *From URL* tab, give your notebook a *name* and an optional *description*, provide the following URL as the *Notebook URL*, and choose the `Python 3.6` environment as the *Runtime*:

```bash
https://raw.githubusercontent.com/Shivam6693/CP4D_workshop/master/Fraud_claim_use_case/fraud-prediction-model_z1EPe81ZI.ipynb
```

![Add notebook name and URL](doc/source/images/wml-2-add-name-and-url.png)

* When the Jupyter notebook is loaded and the kernel is ready then we can start executing cells.

![Notebook loaded](doc/source/images/wml-3-notebook-loaded.png)

> **Important**: *Make sure that you stop the kernel of your notebook(s) when you are done, in order to conserve memory resources!*

![Stop kernel](doc/source/images/JupyterStopKernel.png)


### 2. Run the notebook

Spend some time looking through the sections of the notebook to get an overview. A notebook is composed of text (markdown or heading) cells and code cells. The markdown cells provide comments on what the code is designed to do.

You will run cells individually by highlighting each cell, then either click the `Run` button at the top of the notebook or hitting the keyboard short cut to run the cell (Shift + Enter but can vary based on platform). While the cell is running, an asterisk (`[*]`) will show up to the left of the cell. When that cell has finished executing a sequential number will show up (e.g. `[17]`).

**Please note that some of the comments in the notebook are directions for you to modify specific sections of the code. Perform any changes as indicated before running / executing the cell.**

#### Notebook sections

With the notebook open, you will notice:

- Section `1.2 Install required packages` will install some of the libraries we are going to use in the notebook (many libraries come pre-installed on Cloud Pak for Data). Note that we upgrade the installed version of Watson Machine Learning Python Client. Ensure the output of the first code cell is that the python packages were successfully installed.

![Install required packages](doc/source/images/wml-3.2-install-packages.png)

- Section `2.0 Add Dataset` will load the data set we will use to build out the machine learning model. In order to import the data into the notebook, we are going to use the code generation capability of Watson Studio.
   - Highlight the code cell shown in the image below by clicking it. Ensure you place the cursor below the commented line.
   - Click the 01/00 "Find data" icon in the upper right of the notebook to find the data asset you need to import.
   - Choose the *Files* tab, and pick the virtualized data set that has all three joined tables (i.e. `User<xyz>.POLICIESCLAIMSCUSTOMERS`). Click `Insert to code` and choose `pandas DataFrame`.

![Add remote Pandas DataFrame](doc/source/images/wml-4-add-dataframe.png)

   - The code to bring the data into the notebook environment and create a Pandas DataFrame will be added to the cell.
   - Run the cell and you will see the first five rows of the dataset.

![Generated code to handle Pandas DataFrame](doc/source/images/wml-5-generated-code-dataframe.png)

   - Continue to run the remaining cells in section 2 to explore and clean the data.

- Section `3.0 Create a model` cells will run through the steps to build a model pipeline.
   - We will split our data into training and test data, encode the categorial string values, create a model using the Logistic Regression, Random Forest Classifier algorithm, and evaluate the model against the test set.
   - Run all the cells in section 3 to build the model.

![Building the pipeline and model](doc/source/images/wml-6-build-pipeline-and-model.png)

### 3. Create a Space for Machine Learning Deployments

Before we create a machine learning model, we will have to set up a deployment space where we can save and deploy the model.

Follow the steps in this section to associate a deployment space. 


*    Right Click on the project name in the upper left section of the screen![Project](doc/source/images/wml-3.1.png)
*    Click on the tab where the project is opened
*    Click on Settings tab
![Settings](doc/source/images/wml-3.2.png)
*    Click on Associate a deployment Space
![DepSpace](doc/source/images/wml-3.3.png)
*    Enter fraud_prediction_deployment_space in the deployment space name
![DepSpaceName](doc/source/images/wml-3.4.png)
*    Click on Associate to associate the fraud_prediction_deployment_space deployment space to the project


- Section `4.0 Save the model` will save the model to your project.

- We will be saving and deploying the model to the Watson Machine Learning service within our Cloud Pak for Data platform.

- Update the `MODEL_NAME` variable and provide a unique and easily identifiable model name. Next, update the `DEPLOYMENT_SPACE_NAME` variable, providing the name of your deployment space which was created in [Step 2](#2-create-a-space-for-machine-learning-deployments) above.

![Provide model and deployment space name](doc/source/images/wml-provide-model-and-space-name.png)


Continue to run the cells in the section to save the model to Cloud Pak for Data. We'll be able to test it out with the Cloud Pak for Data tools in just a few minutes!


### 4. Deploying the model

Now that we have created a model and saved it to our respository, we will want to deploy the model so it can be used by others. 

We will be creating an online deployment. This type of deployment will make an instance of the model available to make predictions in real time via an API. 

Continue to run the cells in the section 5 deploy the model to the associated Watson Machine Learning. We'll be able to test it out with the Cloud Pak for Data tools in just a few minutes!
![Deploy](doc/source/images/wml-deploy-model.png)

### 5. Testing the model

Cloud Pak for Data offers tools to quickly test out Watson Machine Learning models. We begin with the built-in tooling.

- Click on the deployment. The Deployment *API reference* tab shows how to use the model using *cURL*, *Java*, *Javascript*, *Python*, and *Scala*. Click on the corresponding tab to get the code snippet in the language that you want to use:

![Deployment API reference](doc/source/images/wml-testing.png)

#### Test the saved model with built-in tooling

- To get to the built-in test tool, click on the Test tab. Click on the `Provide input data as JSON` icon and paste the following data under Body:

```json
{
	"input_data": [{
		"fields": ["capital_gains", "capital_loss", "incident_hour_of_the_day", "number_of_vehicles_involved", "witnesses", "total_claim_amount", "policy_annual_premium", "insured_sex_FEMALE", "insured_sex_MALE", "insured_occupation_adm-clerical", "insured_occupation_armed-forces", "insured_occupation_craft-repair", "insured_occupation_exec-managerial", "insured_occupation_farming-fishing", "insured_occupation_handlers-cleaners", "insured_occupation_machine-op-inspct", "insured_occupation_other-service", "insured_occupation_priv-house-serv", "insured_occupation_prof-specialty", "insured_occupation_protective-serv", "insured_occupation_sales", "insured_occupation_tech-support", "insured_occupation_transport-moving", "insured_hobbies_chess", "insured_hobbies_cross-fit", "insured_hobbies_other", "incident_type_Multi-vehicle Collision", "incident_type_Parked Car", "incident_type_Single Vehicle Collision", "incident_type_Vehicle Theft", "collision_type_Front Collision", "collision_type_Rear Collision", "collision_type_Side Collision", "collision_type_Unknown", "incident_severity_Major Damage", "incident_severity_Minor Damage", "incident_severity_Total Loss", "incident_severity_Trivial Damage", "authorities_contacted_Ambulance", "authorities_contacted_Fire", "authorities_contacted_None", "authorities_contacted_Other", "authorities_contacted_Police"],
		"values": [
			[0, 0, 16, 1, 2, 100210, 1241.04, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1],
			[0, 0, 6, 3, 2, 53730, 1437.53, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0],
			[91900, 0, 22, 4, 0, 71760, 1083.01, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1],
			[0, 0, 10, 1, 1, 70700, 1405.71, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1],
			[0, 0, 16, 2, 2, 44110, 1403.9, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1]
		]
	}]
}
```

- Click the `Predict` button  and the model will be called with the input data. The results will display in the *Result* window. Scroll down to the bottom (Line #114) to see either a "Yes" or a "No" for Churn:

![Testing the deployed model](doc/source/images/TestingDeployedModel.png)

