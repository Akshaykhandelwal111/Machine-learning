# Online-Payments-Fraud-Detection-with-Machine-Learning

The introduction of online payment systems has made payments easier. But at the same time payment methods have also increased. Anyone who uses payment systems is a potential online payment scam especially if you use a credit card pay. This is why detecting onlone payment fraud is so important for credit companies. This ensures that customers are not billed for products or services they nerver pay for. if you want to know how to detect online payments fraud this article will help you. This article describes the task of detecting online payment fraud using machine learning python.

Identifing online payment fraud through machine learning model to classify fraudulent and non fraudulent payments. To do we need a dataset containing information on online payment fraud to understand the types of transactions that lead to fraud. For this work I collected a <a href="https://www.kaggle.com/ealaxi/paysim1/download">dataset</a> from Kaggle contaning historical information about fraudulent transactions for detecting online payment fraud.
<h2>Here are all the columns in the dataset Im using here:</h2>

<b><li>step: represents a unit of time where 1 step equals 1 hour</li>
<li>type: type of online transaction</li>
<li>amount: the amount of the transaction</li>
<li>nameOrig: customer starting the transaction</li>
<li>oldbalanceOrg: balance before the transaction</li>
<li>newbalanceOrig: balance after the transaction</li>
<li>nameDest: recipient of the transaction</li>
<li>oldbalanceDest: initial balance of recipient before the transaction</li>
<li>newbalanceDest: the new balance of recipient after the transaction</li>
<li>isFraud: fraud transaction</li></b>


<h2>I start this task by importing the Python libraries and datasets required for this task:</h2>
import pandas as pd
import numpy as np
data = pd.read_csv("credit card.csv")
print(data.head())
   
<h2>Now, let’s have a look at whether this dataset has any null values or not:</h2>
print(data.isnull().sum())

<h2>So this dataset does not have any null values. Before moving forward, now, let’s have a look at the type of transaction mentioned in the dataset:</h2>
# Exploring transaction type
print(data.type.value_counts())
  
type = data["type"].value_counts()
transactions = type.index
quantity = type.values

import plotly.express as px
figure = px.pie(data, 
             values=quantity, 
             names=transactions,hole = 0.5, 
             title="Distribution of Transaction Type")
figure.show()

<h2>Now let’s have a look at the correlation between the features of the data with the isFraud column:</h2>

# Checking correlation
correlation = data.corr()
print(correlation["isFraud"].sort_values(ascending=False))

<h2>Now let’s transform the categorical features into numerical. Here I will also transform the values of the isFraud column into No Fraud and Fraud labels to have a better understanding of the output:</h2>

data["type"] = data["type"].map({"CASH_OUT": 1, "PAYMENT": 2, 
                                 "CASH_IN": 3, "TRANSFER": 4,
                                 "DEBIT": 5})
data["isFraud"] = data["isFraud"].map({0: "No Fraud", 1: "Fraud"})
print(data.head())

<h1>Online Payments Fraud Detection Model</h1>
<h2>Now let’s train a classification model to classify fraud and non-fraud transactions. Before training the model, I will split the data into training and test sets</h2>

# splitting the data
from sklearn.model_selection import train_test_split
x = np.array(data[["type", "amount", "oldbalanceOrg", "newbalanceOrig"]])
y = np.array(data[["isFraud"]])

<h2>Now let’s train the online payments fraud detection model:</h2>

# training a machine learning model
from sklearn.tree import DecisionTreeClassifier
xtrain, xtest, ytrain, ytest = train_test_split(x, y, test_size=0.10, random_state=42)
model = DecisionTreeClassifier()
model.fit(xtrain, ytrain)
print(model.score(xtest, ytest))

<h2>Now let’s classify whether a transaction is a fraud or not by feeding about a transaction into the model:</h2>

# prediction
#features = [type, amount, oldbalanceOrg, newbalanceOrig]
features = np.array([[4, 9000.60, 9000.60, 0.0]])
print(model.predict(features))




   
