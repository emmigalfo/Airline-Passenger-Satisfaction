# Airline Passenger Satisfaction
Machine learning project predicting customer satisfaction in the aviation industry
![Happy passengers on a plane](./Photos/Happy-passengers.avif)
**Author:** [Emmi Galfo](mailto:emmi.galfo@gmail.com)

## Business Problem 

Airline companies strive to provide excellent customer service. Understanding which factors to focus investments on can increase customer retention, enhance customer experience, increase revenue, reduce costs, and improve brand image. I'm asked to predict which factors are most important for passenger satisfaction and to provide actionable insights for airlines to improve their overall customer experience.  

## Overview 

This classification project looks into which factors contribute most in predicting airline customer satisfaction. By using machine learning modeling to analyze customer data, I was able to predict which factors were most important for passenger satisfaction and provide actionable insights for airlines to improve their overall customer experience. My results showed that online boarding, personal vs business travel, and inflight wifi service had the most impact on overall satisfaction. 

## Data Understanding

For this project, a large dataset regarding airline passenger satisfaction and travel features was downloaded from kaggle.com. 

### Shapes of dataframes:
* Total dataframe before the split:  (129880, 23)
* Training shape: (103904, 23)
* Testing shape: (25976, 23)
* Train/Test split: 0.8/0.2

### Column names and descriptions:
Gender: Gender of the passengers (Female, Male)

Customer Type: The customer type (Loyal customer, disloyal customer)

Age: The actual age of the passengers

Type of Travel: Purpose of the flight of the passengers (Personal Travel, Business Travel)

Class: Travel class in the plane of the passengers (Business, Eco, Eco Plus)

Flight distance: The flight distance of this journey

Inflight wifi service: Satisfaction level of the inflight wifi service (0:Not Applicable;1-5)

Departure/Arrival time convenient: Satisfaction level of Departure/Arrival time convenient

Ease of Online booking: Satisfaction level of online booking

Gate location: Satisfaction level of Gate location

Food and drink: Satisfaction level of Food and drink

Online boarding: Satisfaction level of online boarding

Seat comfort: Satisfaction level of Seat comfort

Inflight entertainment: Satisfaction level of inflight entertainment

On-board service: Satisfaction level of On-board service

Leg room service: Satisfaction level of Leg room service

Baggage handling: Satisfaction level of baggage handling

Check-in service: Satisfaction level of Check-in service

Inflight service: Satisfaction level of inflight service

Cleanliness: Satisfaction level of Cleanliness

Departure Delay in Minutes: Minutes delayed when departure

Arrival Delay in Minutes: Minutes delayed when Arrival

Satisfaction: Airline satisfaction level(Satisfaction, neutral or dissatisfaction)

#### Numerical Columns- Training data

![Heatmap of correlations](./Photos/Heatmap.png)

Top correlations as seen in the heat map:
* Departure delay and arrival delay are the most strongly correlated.
* Next is ease of online booking and inflight wifi service.
* Finally, cleanliness is correlated with food and drink, seat comfort, and inflight entertainment. 

#### Categorical Columns- Training data

![Categorical bar graph](./Photos/categorical-bar.png)

We can see that there were more loyal customers than disloyal customers, more business travelers than personal travelers, and more business and eco seats than eco plus seats.

#### Target- Training data

![target bar graph](./Photos/Target-bar.png)

We can see from the graph above that there are fairly equal amounts of satisfied and neutral/dissatisfied customers. 

## Modeling
The goal of the machine learning modeling is to identify the factors that are most important for predicting passenger satisfaction and to develop a model that can accurately classify passengers as either satisfied or dissatisfied.

The approach to this problem is to create a model that minimizes False Positives so that it does not predict many passengers are satisfied when they are not.  The model will be evaluated using a precision score. This explains out of all the satisfied predictions how many are correctly labeled. 

### Model 1: Logistic Regression 

Lets start with simple logistic regression classifier.

__Test Results:__
* precision score: 0.87
* recall score: 0.87
* f1 score: 0.87

![Confusion matrix](./Photos/model_1_confusion_matrix.png)

The initial model returned a precision score of 87%. It is important that the dissatisfied customers are labeled correctly. The top two boxes on the table show all of the dissatisfied customers and are split based on whether the model predicted them correctly or not. The initial model mislabeled 1,434 dissatisfied customers. 

### Final Model: XGBoost
The final model used is an XGBoost Classifier. After a grid search, parameters were tuned to random_state=42, learning_rate=0.01, n_estimators=1000, max_depth=7. 

__Test Results:__
* precision score: 0.97
* recall score: 0.96
* f1 score: 0.96

![Confusion matrix](./Photos/Final_Model_confusion.png)

The final model had a precision score of 97%! This is much better than the initial score. The final model only mislabelled 306 dissatisfied customers. 

The final model looks good, next steps include pulling out the important features and making recommendations. 

![Important features bar chart](./Photos/feature-importance.png)

When looking at the top important features, all that can be determined is the imact they have. The graph does not explain whether these impacts are positively or negatively correlated with satisfaction. However, the coefficients from the logistic regression model can be used to help determine the relationship between the feature and the satsifaction of a customer. 

Top 3 features from XGBoost and their corresponding logistic regression coefficients:
* Online boarding: 0.82
* Personal travel: -2.71
* Inflight wifi: 0.52

Based on the logistic regression results, online boarding and inflight WiFi are positively correlated with satisfaction, while personal travel is negatively correlated.

![Top-Feature-Online-Boarding](./Photos/Online-boarding.png)

The feature that had the highest impact by far on customer satisfaction was online boarding.  Passengers ranked their satisfaction of online boarding from 0-5 with 5 being the most satisfied. On the graph, customers who were satisfied with their overall experience are shown in green.  The graph shows that as the customers ranked online boarding higher they were more likely to be satisfied overall. 

![Top-Feature-Type-of-travel](./Photos/type-of-travel.png)

Personal travel was the second most important indicator of overall satisfaction. The graph shows that personal travel is highly correlated with overall dissatisfaction.  

![Top-Feature-WiFi](./Photos/WiFi-Service.png)

The third highest indicator of overall satisfaction was inflight WiFi service. Higher ratings of wifi service satisfaction tended to result in higher levels of overall satisfaction. 

## Results and Conclusions

***Results*** 
* The final and best model was an XGBoost classifier. 
* The model had a precision score of 97%
* The model's predictions resulted in false negatives only 2% of the time. 
* The top 3 features that had the most impact on predicting passenger satisfaction were the following:
    * online boarding
    * personal travel
    * inflight wifi service

* The 3 features that had the least impact on predicting passenger satisfaction were the following:
    * gender
    * departure delay
    * food and drink

***Insights and Recommendations***
1. Online boarding streamlines the check-in and boarding process. It offers customers a convenient way to check-in, pay for bags, reserve seats, and obtain boarding passes. This is the number one feature that customers valued. Putting resources into online boarding to keep it state of the art may go a long way in terms of improving customer satisfaction. 
2. There are two main reasons to travel, personal and business. When customers were traveling for personal reasons, they were more likely to be dissatisfied with the experience. This may stem from who is paying for the travel-- personal payees versus corporate payees. The action item is looking deeper into what factors contribute to their dissatisfaction. This is a good area to look at further. 
3. The third most important feature in predicting satisfaction was the inflight wifi service. My recommendation is to put resources into exploring opportunities to optimize the WiFi service during flight. 

***Limitations***
* Data bias- dataset may not be representative of the general population.
* Limited feature set- There may be important features not accounted for such as weather. 
* Changes in data distribution- if customer preferences or behaviors change over time, the model may need to be updated to reflect these changes.

***Next Steps***
* Explore which features are most important to personal travelers.
* Expand the data set to include more airlines and/or more features.

***
Thank You!
emmi.galfo@gmail.com
***

## Repository structure: 
├── AirlineDataSet : data used in project \
├── Photos : images used in readme, presentation, and notebook \
├── Airline_Passenger_Satisfaction.ipynb : jupyter notebook used to create project \
├── README.md : project summary and conclusions \
├── presentation.pdf : stakeholder powerpoint slides 
