
# Azure AutoML v/s Azure HyperDrive!
This is the capstone project of Udacity's Machine Learning Engineer with Microsoft Azure nanodegree. The goal is to compare models built using Azure's AutoML and HyperDrive on a given dataset on the basis of performance on the user-specified metric(accuracy in this case).

## Dataset

### Overview
The data contains details of a bank's customers and the target variable is a binary variable reflecting the fact whether the customer left the bank (closed his account) or he continues to be a customer. There are 12 different variables used to predict the churn of a customer, and the data contains around 10,000 such instances.
The data can be found [here.](https://www.kaggle.com/shrutimechlearn/churn-modelling)

### Task

The dataset is used for classification i.e assigning pre-defined labels on the basis of the available features.The project solves the task of finding labels indicating the churn status of a customer of bank(1 for churn and 0, otherwise). Then it compares the two different methods used on the basis of accuracy achieved while training the models.(accuracy - fraction of correctly predicted labels).
The features of the dataset on which the models are trained are:

* Surname: The surname of the customer.
* CreditScore: Indicating consumer's creditworthiness(out of 850, the higher the better).
* Geography: Country of origin of the customer.
* Gender: male or female.(binary feature).
* Age: age of the customer.(continous feature).
* Tenure: the period or term of association with the customer.
* Balance: Customer's current account balance.
* NumOfProducts: Number of products the customer has purchased or is engaged with.
* HasCrCard: Binary feature indicating whethe the customer holds a credit card(1-yes,0-no).
* IsActiveMember: Indicating the current status of the customer as per the bank policies.
* EstimatedSalary: Predicted salary of the customer.
* Exited: Churn Status(1 for yes, 0 for no). This is the target variable.

### Access
The data is first preprocessed (Label and One-Hot Encoding is applied) and uploaded to the project github repository and is then accessed using the TabularDatasetFactory module available in the Azure's dataset_factory using the uri of the dataset. Then it is converted into pandas DataFrame using the to_pandas_dataframe() function.
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(39).png)
<br>

## Automated ML
AutoML settings and confuguration used:<br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(38).png)
<br>
Explanation: Since we had to assign labels to the new data using the model trained on the training data, the 'task' is set to classification. There are many evaluation metrics for classification tasks but here we have chosen 'accuracy' as our primary_metric(the model will run to maximize the primary_metric).
Then the training data(as a TabularDataSet) and the target column('Exited) was assigned. The compute_target was set to the cluster that was specially created for this project. The 'n_cross_validations' and 'iterations' parameters were set randomly(7 and 5 respectively) on the basis on intuition, and can be tuned as per requirements in order to get varying results.

### Results
*TODO*: What are the results you got with your automated ML model? What were the parameters of the model? How could you have improved it?

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters.

## Hyperparameter Tuning
*TODO*: What kind of model did you choose for this experiment and why? Give an overview of the types of parameters and their ranges used for the hyperparameter search


### Results
*TODO*: What are the results you got with your model? What were the parameters of the model? How could you have improved it?

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters.

## Model Deployment
*TODO*: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:
- A working model
- Demo of the deployed  model
- Demo of a sample request sent to the endpoint and its response