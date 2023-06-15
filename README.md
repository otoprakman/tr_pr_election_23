# Data Analysis for 2023 Turkish Presidential Elections

## Introduction

Presidential elections were held in Turkey in May 2023, alongside parliamentary elections, to elect a president for a term of five years. It marks the first time a Turkish presidential election has gone to a run-off.

In the first round, Erdogan and Ogan outperformed expectations to receive 49.5% and 5.2% of the vote respectively. Meanwhile, Kilicdaroglu won 44.9%, while Muharrem Ince (who remained on the ballot despite withdrawing) received 0.4%. Since Erdogan's vote share was 0.5% short of winning outright, he and Kilicdaroglu contested a run-off vote on 28 May. Erdogan was re-elected in the second round with 52.18% of the vote to 47.82% for Kilicdaroglu. 

**(Please consider that this blog post is not written by a Subject-matter expert. All shared information is based on the resources mentioned at Resources section and author’s personal experience. Main purpose of this post is to share personal thoughts regarding the critical elections)**

Election results were disappointment for Kilicdaroglu supporters and Erdogan has all the advantage going into the second round after winning 49.5 percent of votes in the first round. 5 percent of the vote was garnered by Ogan who is a nationalist and at the runoff it was crucial for both leaders Erdogan and especially for Kilicdaroglu. However, there was another tackle for Kilicdaroglu, Kurdish votes who supported Kilicdaroglu in the first round. Kilicdaroglu had to seek the balance of nationalist statements and the level of his tone. He chose nationalist one. How did it affect the second round, let's have a look.

I'll provide some questions and answers by providing some graphs and analysis.

## Testing the call of KK to his supporters to vote.

Question: Is there a significant turnout rate difference in cities where KK get the most of the vote vs RTE?

<img src="https://render.githubusercontent.com/render/math?math=Decline\ Ratio = \frac {\sum{Successful\ Authorization\ Attempts}} {\sum{Authorization\ Attempts}}">

There are 30 cities where KK was leading in the first round and 51 cities where RTE leading. Here in the plot we compare pct of turnout changes where candidates were leading in the first round. According to the plot above KK's call of vote was not responded well by voters.

![hour_day](https://user-images.githubusercontent.com/53580699/133819745-9ef91e3a-5db1-4817-9680-ebb682580836.png)

Here we observe that there are high decline ratio at early hours in a day. One of the reason could be it is unlikely to transaction happen so it could be seen as suspicious.

![Rplot](https://user-images.githubusercontent.com/53580699/133821494-7f67ad33-62a5-4cc7-ace4-06b0397f5ca9.png)

In this plot, we observe that there is difference between days of a week in terms of the decline ratio. Proportion of transactions for each day are displayed on each point.

![Rplot01](https://user-images.githubusercontent.com/53580699/133825824-c14bc2c0-2b40-49d7-be09-0319b8c33b2a.png)

We wanted to observe if decline ratio is related with the day in a month. For example, most of employees get their wages at 15th or 1st day of the month, therefore there might be a relation between this fact and decline ratio.

### Association Rule Mining

Association Rule Mining is mainly used for identifying interesting relations between variables. One example is the market basket analysis that discovers which items are more likely bought together. In our problem, we have transaction data and most of variables are categorical. Generating association rules will help us to better understand or reveal hidden patterns for different transactions. Apriori algorithm is used for generating rules and presented below.

![dec_11](https://user-images.githubusercontent.com/53580699/134386579-726da76e-de4b-4ff1-9f21-bd1b440f9e21.png)

We can observe Confidence values which is the conditional probability of rhs given lhs. For example;

<img src="https://latex.codecogs.com/gif.latex?P\left(Transaction\_Status=Declined&space;\;\middle|\;VAR\_D=50\right)&space;=&space;0.89\" title="P\left(Transaction\_Status=Approved \;\middle|\;VAR\_D=50\right) = 0.89\" />

There are many other rules that are not presented in this blog, it is advised to use this method for better understanding relations between variables.

## Feature Engineering

We can generate new variables using timestamp feature and also changes of variables over time. Here are some features that can be generated;

- Timediff_sec: numeric, time between consecutive transaction in each billing series
- Time_until_success: numeric, time between the first attempt and the last approved attempt
- Flag_Vars: binary [0,1] if card information changed in a billing series
- Response_F: binary [0,1] if Response changed in a billing series
- Day: integer [0:31] day of transaction
- Hour: integer [0:24] hour of transaction
- wDay: integer [0:6] day in a week that transaction happened

## Modelling

In this section, recurring authorization attempts are considered for the modelling. We are going to model the problem with 2 different approaches. One of them is the classification in which Transaction Status(Approved/Declined) is used as a target variable and the approval likelihood will be estimated over next 10 days. The other approach is the regression in which time_until_success will be target variable and it will be forecasted.

### Classification

Random forest is selected as base model because it can handle categorical variables and can generate probability estimates using "ensemble" nature of the method. 

![class1](https://user-images.githubusercontent.com/53580699/134405412-d260cc1d-2f95-4bde-9350-ce37d99616d1.png)

True-Negative is the total number of saved unauthorized attempts. False-Negative is the total number of missed opportunity for authorized attempt.
Dataset is imbalanced and probability threshold can be analyzed for further analysis.

![class2](https://user-images.githubusercontent.com/53580699/134405622-87fa130b-1cdd-44a8-8997-5c11d62e51bf.png)

We can evaluate our base model in a following way;
In the plot above each point represents the actual retries and y-axis is the predicted probabilities from our model. There are multiple attempts for some days because some attempts are tried on the same day. If we assume that retry probability threshold is 0.5 then it can be said that 4th, 5th and following attempts that are below 0.5 could be omitted. In other words, 13/23 attempts would be ignored and saved. However 1st, 2nd and other attempts would be retried again redundantly. 

### Regression

In this approach, time to approval is forecasted based on the available variables. Downside of this approach is the difficulty of evaluating the model performance based on the historical data. As the timeline below, model predictions will probably either Predicted-1 or Predicted-2. We don’t know if the Predicted-1 is correct, on the other hand if there is already any retry before that time then we can clearly say that number of declined attempt is reduced. We don’t know if the Predicted-2 is correct as well and We clearly see that time to approval is increased. However number of declined authorization attempt declined.

![reg1](https://user-images.githubusercontent.com/53580699/134407046-82f07e9d-dcd0-4f23-9a1e-fe6584002bc5.png)

In this approach there are two conflicting objectives minimizing total number of attempts and time to approval. Our model's purpose is to identify time to approval and by doing that minimize the redundant authorization attempts. 

![reg2](https://user-images.githubusercontent.com/53580699/134407048-d53bc3b3-fa96-4928-a2f2-bcdbff1791b6.png)

According to the test set and model predictions.

- Total Unique Billing Series: 2150 
- Predicted-1: 969 (MAE : (-)625942.6 sec) (RMSE: 2283714) (Total Declined Attempts: 683/1737)
- Predicted-2: 1181 (MAE : (+)10987.86 sec) (RMSE: 26785.41) (Total Declined Attempts: 683/1737)

![reg4](https://user-images.githubusercontent.com/53580699/134419241-430d2d6e-ed14-4061-ac18-cb8afe13719f.png)

As an example, we can't say predicted time is correct, however if we applied this approach we could omit 4 redundant attempts. In our test set 1737 attempt could be ignored and our model's result suggests 683 of them could be saved.

![reg3](https://user-images.githubusercontent.com/53580699/134407051-d9119fa0-5295-4446-90d5-171738fb5fc8.png)

Random forest is an ensemble of multiple decision trees and each trees has its own predictions. Instead averaging decision tree results, we can estimate the probability density function using non-parametric kernel density estimation. First plot above is an example of Predicted-1 and second plot is the example of Predicted-2. 

## Conclusion

In this blog post, we introduced one of the common problems in payment analytic and proposed two different approaches. As a future research direction, number of variables can be introduced to the models like, type of merchant, customer profiles, country of the customer, type of the prouct etc. Another methods like multi-armed bandit or more sophisticated tree based algorithms can be used. Sampling bias is a critical problem and should be considered before modelling.

## Resources
1) https://medium.com/disney-streaming/building-a-rule-engine-that-helps-to-optimize-recurring-churn-failures-at-scale-in-disney-a6b41bc6614b
2) https://recurly.com/blog/what-are-recurring-payments-and-subscription-billing/
3) https://recurly.com/blog/how-data-science-work-reveals-hidden-trends-in-payment-success-rates/
4) https://recurly.com/blog/the-real-meaning-of-churn/
5) https://docs.recurly.com/docs/retry-logic
6) https://stripe.com/en-au/guides/optimizing-authorization-rates
7) https://www.adyen.com/blog/optimizing-payment-conversion-rates-with-contextual-multi-armed-bandits
8) https://www.adyen.com/blog/Rescuing-failed-subscription-payments-using-contextual-multi-armed-bandits
9) https://medium.com/paypal-ai/using-machine-learning-to-improve-payment-authorization-rates-bc3b2cbf4999

