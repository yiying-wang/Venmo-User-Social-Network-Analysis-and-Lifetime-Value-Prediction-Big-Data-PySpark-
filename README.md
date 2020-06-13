# Venmo-User-Social-Network-Analysis-and-Lifetime-Value-Prediction-Big-Data-PySpark-
## Executive Summary
Venmo is one of the most popular peer-to-peer (P2P) mobile payment apps in U.S. It is popular for its social flavor that users are required to make transaction with a message describing what the transaction is about. In this report, we conducted text analytics, social network analytics, and predictive analytics to extract insights from Venmo transaction data. For text analytics, we extracted emojis and words from descriptions, then we matched them with corresponding topics using dictionaries. We found that different users have different spending behavior, which usually stabilized in about a month. For social network analytics, we counted the user’s friends and friends of friends and calculated clustering coefficient and PageRank. For predictive analytics, we utilized the RFM model and analytics above to run regression model to predict the number of future transactions of users. We found that the social network metrics combined with the spending profile have the highest predictive power. The user with higher PageRank and more friends will have more transactions, which highlighted the unique feature of Venmo, sharing when transacting. As a result, we suggested Venmo to emphasize its social flavor and to encourage its user to expand their social networks.
## Introduction
Venmo is a peer-to-peer (P2P) mobile payment app owned by PayPal. It allows its users to exchange money with a click of a button. In the fourth quarter of 2019, Venmo has about 30 billion U.S. dollars volume with more than 40 million active accounts. What made Venmo so popular is that users are required to make their transaction with a message describing what the transaction is, which transforms transactions into sharing experiences. In this report, we will analyze the transaction data in three aspects. First, we applied text analytics to establish users’ profiles. Then, we applied social network analytics to investigate users’ networks. Finally, we used regression to predict the number of future transactions.
Data Characteristics
The transaction dataset has millions of transaction records from thousands of users. It includes the following features, user1, user2, datetime, transaction_type, description, is_business, and story_id. For text analytics, the focus will be description. We transformed descriptions into topics with emoji and word dictionary to better understand what the transaction is about. It is worth mentioning that we added some additional words to the default dictionary. For example, we added ‘weee’, a popular online food delivery service, into ‘Food’. For social network analytics, the focus will be user1 and user2, we will use the combination of user1 and user2 to identify the user’s friends as well as friends of friends and calculate social network metrics. For predictive analytics, we will use the RFM model as well as text and social network analytics to develop regression model to predict the number of future transactions.

  
### Text Analytics
The focus for text analytics is the description. As we mentioned, the description is a message shared by the user describing what the transaction is about. It is usually emoji, text, or a combination of both.
First, we extracted emojis and words from descriptions, then we matched them with corresponding topics using dictionaries. Sometimes, there are multiple emojis and words in one description, so we assigned a score of 0.7 for each word topics and a score of 0.3 for each emoji topics, and then we added up all the scores and chose the topic according to the highest score. Here are some interesting findings. Users like sending emojis when sharing experiences, and there are more than 25% of descriptions are emoji only. It seems that users are having a good time when sending emojis. The most popular emoji topics are Food, People, and Activity, and the most popular emojis are pizza 🍕, cheers 🍻, flying money💸, wine 🍷, and party popper 🎉.
  
 Then, we tried to establish user profiles. The static user profile identified the proportion of each type of spending for each user. For example, it is showed that user 2 spent most of the money on Food, and user 4 has a diverse set of different types of spending, Event, Food, People, Travel, and Utility. The dynamic user profile explored how the proportion of each type of spending change over time. We calculated the average and standard deviation across all users and excluded values that are zero. It is showed that the average percentage of different types of spending usually stabilized in around a month.

     
### Social Network Analytics
We will use the user1 and user2 to identify the user’s friends as well as friends of friends and calculate social network metrics.
Friends are defined as other users who had transactions with the user. First, we union user1 and user2. Then, we selected distinct combinations. The computational complexity is 2n.
Friends of friends are defined as other users who had transactions with the user’s friends but not the user. First, we left join user table with user table, and user1 as the original user, user2 as friends, and user3 as friends of friends. Then, we excluded the rows where user3=user1 and user3 =user2. The computational complexity is O(M+N).

 We successfully identified users’ friends as well as friends of friends. Then, we calculated the clustering coefficient and the PageRank to better capture their social network.
The Clustering Coefficient measures how connected a vertex’s neighbor is to one another. In other words, if your friends all know each other, you have a high clustering coefficient. More specifically, it is calculated as the number of edges connecting a vertex’s neighbors divided by the total number of possible edges between the vertex’s neighbors. We calculated the clustering coefficient as friends count divided by the friends of friends count.

The PageRank is best known as the core metric behind Google’s search engine. It includes three distinct factors, the number of vertices that link to the target, the PageRank centrality of the linking vertices, and the link propensity of the linking vertices. In other words, your PageRank will increase if you have more friends, if your friends have high PageRank, and your friends do not have many other friends. We used GraphX to calculate the PageRank.

### Predictive Analytics
One of the biggest questions in Customer Relationship Management (CRM) is to predict the number of future transactions. We predicted the total number of transactions a user will have by the end of their first year in Venmo based on transaction dataset as well as text and social network analytics above.
First, we created a dependent variable Y, which is the total number of transactions by the end of the first year. Then, we created the recency and frequency variables. Recency refers to the last time a user was active, and frequency refers to how often a user uses Venmo in a month. It is worth mentioning that if there is no transaction, the recency will be 30 and the frequency will be 0. Finally, we developed multiple regression models and plotted the accuracy of the model for each lifetime point.



## Conclusion

We started with establishing the user’s static and dynamic profile. It is showed that different users have different spending behavior, which usually stabilized in about a month. Additionally, user’s enjoying sending emojis. Then, we tried investigating the user’s social networks. We counted the user’s friends and friends of friends and calculated clustering coefficient and PageRank. Finally, we utilized the RFM model and analytics above to run regression model to predict the number of future transactions of users. It is worth mentioning that we developed model for each lifetime point. We discovered that more recent month’s data of recency and frequency could better predict the outcome for the year. We also discovered that the social network metrics combined with the spending profile have the highest predictive power. The coefficients indicate that the user with higher PageRank and more friends will have more transactions, which highlighted the unique feature of Venmo, sharing when transacting. Based on these analytics, we suggested Venmo to emphasize its social flavor and to encourage its user to expand their social networks. Additionally, Venmo could have more business partnerships, so that users could have a more diverse spending profile. With these methods, Venmo could enhance its user stickiness, have more active accounts, and achieve higher volume.
