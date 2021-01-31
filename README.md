
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
The data is first preprocessed (Label and One-Hot Encoding is applied) and uploaded to the project github repository and is then accessed using the TabularDatasetFactory module available in the Azure's dataset_factory using the uri of the dataset. Then it is converted into pandas DataFrame using the to_pandas_dataframe() function.<br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(39).png.jpg)
<br>

## Automated ML
AutoML settings and confuguration used:<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(38).png.jpg)
<br>

Explanation: Since we had to assign labels to the new data using the model trained on the training data, the 'task' is set to classification. There are many evaluation metrics for classification tasks but here we have chosen 'accuracy' as our primary_metric(the model will run to maximize the primary_metric).
Then the training data(as a TabularDataSet) and the target column('Exited) was assigned. The compute_target was set to the cluster that was specially created for this project. The 'n_cross_validations' and 'iterations' parameters were set randomly(7 and 5 respectively) on the basis on intuition, and can be tuned as per requirements in order to get varying results.

### Results
In our experiment we found out ensembling machine learning models give the best results, with VotingEnsemble giving the highest training accuracy of 0.864. Other models like, StackEnsemble and XGBoostClassifier also gave almost similar results, but they might have overfitted the data as the data des not have many instances.

The parameters for Voting Ensemble were: 
boosting_type' as 'gbdt', 'colsample_bytree' = 1.0, 'min_child_weight' = 0.001, 'n_estimators' = 100, 'n_jobs' = 1, 'num_leaves' = 31, 'silent': True, 'subsample' = 1.0, 'subsample_for_bin' = 200000, 'subsample_freq' = 0, 'verbose' = -10, 'importance_type' was 'split', 'learning_rate' = 0.1 and 'min_child_samples' = 20.

![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(45).png)
<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(46).png)
<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(47).png)
<br><br>

#### Scope of improvement:

1. Model explainability is an area which is not given attention. Working on it will give better options to pick a model in the future, especially when working on real-life cases. 
2. Option to whether enable deep learning or not should lie solely in the hands of the user.

## Hyperparameter Tuning
For this part, the Logistic Regression model, of the SKLearn library was used as the base model. The train.py file was generated, which contains the code for the model, parameters used, data cleaning and splitting the data into train and test sets.

Parameters used:
* BanditPolicy as the termination policy(evaluation_interval = 2, slack_factor = 0.1,delay_evaluation = 1).
Benefits of the early stopping policy: An early termination policy is quite helpful when the run becomes exhaustive. It ensures that we don't keep running the experiment for too long and end up wasting resources and time, in order to find what the optimal parameter is. A run is cancelled when the criteria of a specified policy are met. Examples of early termination policies we can use are - BanditPolicy, MedianStoppingPolicy and TruncationSelectionPolicy . In our project we have used BanditPolicy as the early termination policy.This early termination policy is based on the slack factor and delay evaluation. It basically checks the job assigned after every 'n' number of iterations(n is passed as an argument). If the primary metric falls out of the slack_factor, Azure ML terminates the job.
<br>

* RandomParameterSampling as the parameter sampler(--max_iter" : choice(5,10,20,40),
        "--C": uniform(0.5,1.0)
)
Benefits of the parameter sampler: Choosing the right parameter sampling method is necessary as well a beneficial step to follow as it can have visible affects on your run time. Parameter sampling means to search the hyperparameter space defined for your model and select the best values of a particular hyperparameter. Azure supports three types of parameter sampling - Random sampling,Grid sampling and Bayesian sampling. RandomParameterSampling supports discrete and continous hyperparameters. In random sampling, hyperparameter values are chosen randomly, thus saving a lot of computational efforts.It can also be used as a starting sampling method as we can use it to do an initial search and then continue with other sampling methods.
<br>

* max_total_runs = 20
* max_concurrent_runs = 4

### Results
With the above parameters, we achieved an accuracy of 0.8035, which is lesser than what we achieved in the AutoML experiment.<br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(37).png)
<br><br>

#### Scope of improvement:

1. Option to tune different models should also be provided, not just hyper-parameters. 
2. The reason behind a run failure of an experiment is not shown as an output. This should also be taken care of as it becomes a tedious task searching the error manually line by line.
3. Logging can be made more descriptive.
<br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(40).png)
<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(41).png)
<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(42).png)
<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(43).png)
<br><br>
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(44).png)
<br><br>

## Model Deployment
The experiment which gave the best result was to be deployed. Since, Azure's AutoML outperformed experiment using HyperDrive, we deployed the former experiment.

To deploy the experiment:
* Environment and configuration files were created.
![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(48).png)
<br><br>

* Azure Container Instance was used for deployment (with cpu_cores = 1 and memory = 1 gb).

##### The model is deployed and the status is healthy:

![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(49).png)
<br><br>
 
We used the endpoint's scoring uri to test on sample data which was new to the trained deployed model.

![alt text](https://github.com/himanshu004/AZMLND_Capstone/blob/main/images/Screenshot%20(50).png)
<br><br>

and achieved a testing accuracy of 80%.


## Screen Recording
Click [here](https://youtu.be/TdtnoNcarOg) to see the ScreenCast.
