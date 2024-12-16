---
weight: 2
title: "Applying ML Models to Flipping Properties Business"
subtitle: ""
date: 2024-06-06T21:06:12+02:00
lastmod: 2024-06-06T21:06:12+02:00
draft: false
author: "Xiaochen"
authorLink: "/about/"
description: "Applying ML Models to Flipping Properties Business"
license: ""
images: []

tags: ["Decision Theory","Properties"]
categories: ["Machine Learning"]

featuredImagePreview: "https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/202412170141233.jpg"
featuredImage: "https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/202412170141233.jpg"

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
math:
  enable: false
  # ...
mapbox:
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---

<!--more-->
## 1 Background
**Flipping** is a real estate investment strategy where an investor purchases a property intending to sell it for a profit in a short period. Beattie (2022) states, "It is significant to complete the transaction as quickly as possible.  This limits the time that your capital is at risk,  such as mortgage, utilities, property taxes, insurance, and other costs associated with homeownership."

{{< admonition tip "Understand Two Main Reasons Led to the Failure of Zillow iBuyer Program (Dave Ramsey Team.; Barber, n.d.)" >}}
 
**Financial issues**
The housing market has been wildly unpredictable—especially since the pandemic, and the imbalance of demand and supply has shot up house prices. A large part of iBuying is flipping homes for resale. But supply-chain and labor issues are slowing down renovations. So Zillow was having trouble getting the massive number of homes they purchased back on the market fast enough—which led to huge revenue losses.
 
**Algorithmic shortcomings**
In an attempt to catch up with top iBuying competitors, ML/business management pushed for aggressive buying in response to conservative recommendations of the algorithm. In other words, they were pushing the business into paying too much for too many homes.
{{< /admonition >}}

## 2 Recommendation  
For the two reasons mentioned above, we improve the models. Machine learning and traditional models can be used together to ensure we practice it properly in the real world:
1.	**Decreasing transaction frequency**  

Compared to previous high-frequency transactions, we decrease the “buy” actions largely and focus on the estates whose values are potentially increasing.
2.	**Maximum Profits**  

Instead of the “Aggressive” algorithm, we compare the baseline and ML models and adopt the minimum price between the two. That is, we buy potential high-valued estates with lower prices to make more profits. 

## 3 Predicted Outcomes

We will cancel nearly 66% of transactions and keep 34% left. However, these transactions will bring a higher average profit (from 20,802.5 to  192,172.0) and the average profit will increase by eight times. The total number of houses available in the market is 20 million (5%\*400,000,000=20,000,000) every year. 

The maximum profit of the baseline model is 416 billion (20,802.5*20,000,000) if we can buy and sell all the houses. In contrast, the maximum total profit of the combined model will reach 1,307 billion, but we only fight for 34% of the market. Moreover, if we focus on a few transactions, we can develop more global markets to get more market shares. 

In addition, we have enough resources to provide better products and services to our customers. Hence our company will attract more high-quality customers that help improve profit margin.   

## 4 Building the Combined Model

**1. Data Management Procedures**

**Review of The Data**
This data sample contains 58222 observations with 35 features. All columns are of integer type (int64) and there are no missing values in any of the columns.  The columns REGION,  METRO, TYPE, and BUILT are categorical features. Columns like VALUE, CONDO, BATHS, UNITSF, LOT, AMTX, ROOMS, DINNING, CELLAR, GARAGE, etc. stand for various characteristics of housing units. The data also contains encoded features that likely represent conditions or features of housing (1 or 2), such as AIR, CLIMB, ELEV, COOK, etc. Negative values (-6, -7, -8, -9), which indicate special coding for not applicable data, unknown, refused, or not reported. From the correlation heat map on the right, we can see some features like the correlation between EROACH and EVROD is very heavy.

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/202412170142816.png)

**Preprocessing and Feature Engineering**
The number of -6 in features LVALUE, INCP, CLIMB, MOBILTYP, ELEV, and FRSTOC is more than 50%, so we exclude these features.  VALUE is our target feature, which will use the ML model to predict. The values of AMTX, and AMTI are proportional to the house price and related to target leakage. As a result, AMTX and AMTI have to be removed. Then we delete all the rows where negative values in any columns. At last, we keep 41512 observations. Since the values of categorical and encoded features in this dataset have been transformed into numbers, we do not need encoding and numerical transformations here. 

**Employed Data Partitioning and Justification**
We divide the dataset with 80% training data and 20% holdout data. We use Cross-Validation to partition data: The training set is split into 5 sets that are used both for training and validating. This is useful when the dataset is small, and maximizes the amount of data available for training.

**Technical/Data Issues**
The current practice formula uses the value of CLIMB to calculate the predicted value, but in the dataset, more than 90% of data is -6, which means not applicable data. For now, we use the median of observations with effective values, and the value is 1. 

**2. Modeling Procedures**

**Model Selection Process**
The target feature is VALUE (Predicted House Price), so a regression model will be built. We do it in two ways.

•	Python A series of regression models are defined. Each model is trained on the data and its performance is evaluated (see table and figure below). The best model is Gradient Boosting Regressor.

| Model| R2 Score | Adjusted R2 Score |	Cross Validated R2 Score |	RMSE|
|----------|----------|----------|----------|----------|
|LinearRegression|	0.307718182|	0.305543037|	0.312510664|	265152.4945|
|DecisionTreeRegressor|	-0.253739748|	-0.257678991|	-0.101007379|	356827.1998|
|KNeighborsRegressor|	0.077789257|	0.074891677|	0.048375912|	306033.7634|
|MLPRegressor	|0.139274147	|0.136569751|	0.139765329|	295655.9874|
|RandomForestRegressor|	0.415025563|	0.413187576|	0.434401922|	243737.6944|
|**GradientBoostingRegressor**|	**0.451034853**|	**0.449310008**|	**0.448503465**|	**236116.6651**|
|SVR|	-0.071740704|	-0.075108107|	-0.074265871|	329912.7631|
|XGBRegressor	|0.406049595|	0.404183406|	0.440892681|	245600.556

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/202412170142760.png)

**Optimal Number of Features**
Then we use GridSearchCV to optimize the number of features. Creating a KFold object with 5 splits and specifying a range of hyperparameters (n_features_to_select: from 10 to 20). The result is shown in the figure on the left below. We choose 13 as the number of n_features_to_select to train the model again and the final performance is shown in the figure on the right below.

| Optimal Number of Features                     | Final Performance                     |
|------------------------------------------|------------------------------------------|
| ![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/202412170143373.png)                  | ![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/202412170143287.png)                 |

•	Automated Tool- DataRobot  25 features are regarded to be informative and selected to build the model. The best model is Random Forest  Regressor. The cross-validated R2 Score is 0.4920. 

**3. Evaluation and Valuation Process**

**Feature Effects**
On the one hand, from DataRobot, we can find the first 6 important features that will influence the house price are REGION, UNITSF, BATHS, BUILT, ROOMS, and  LOT. On another hand, for the current practice formula, the features are UNITSF, BATHS, BUILT, ROOMS, LOT, and CLIMB. 5 features are the same. REGION is always one of the most crucial factors when we are going to trade a property because properties in desirable areas, close to amenities, good schools, and lower crime rates, tend to attract more buyers, driving up prices. The result of the ML model is consistent with our common sense.  Other features are essential for a house and impact people’s daily lives, therefore, impacting the price as well.

**Combined Model**
We calculate the 4 models and the details of different models’ results are shown in the table below. Because the ML models are used to predict the house price, the predicted values are intended to be close to the real value of the property. That means the offer prices from the ML model are usually higher and the profit room of the ML Model is usually lower. 

| Model              | Transactions Counts | Sum Profit         | Average Profit  |
|--------------------|---------------------|--------------------|-----------------|
| Baseline Model     | 8302               | 172,702,670.2      | 20,802.5        |
| ML Model (Py)      | 2949               | 8,534,831          | 2,894.1         |
| ML Model (DR)      | 2814               | 10,299,172.3       | 3,660.0         |
| Combined Model     | 2814               | 540,771,939.5      | 192,172.0       |

**The Strategy of Combined Model**
For a potential transaction, when we combine baseline and ML models, the minimum purchase price can be obtained from the minimum between the two models. Meanwhile, only 0.9*the predicted price (10% for profit margin) is greater than the purchase price can we take a “Buy” action.  The strategy is designed for buying potential high-value properties with lower prices to guarantee profit.  

**Other Key Modelling Issues**
The current practice formula in the baseline model is 
Ln(VALUE) ~ 12.751 + 22.5\*(UNITSF/1,000,000)-78.9\*(LOT/1,000,000,000)+ 0.136\*ROOMS –
BUILT/1000 +0.34\*BATHS +0.027*CLIMB.  The value of CLIMB is not provided in most observations, so we should update the formula to satisfy the real situation.

*Reference*

Beattie, A. (2022, August 26). 10 Steps for Successfully Flipping a House. Investopedia. https://www.investopedia.com/articles/mortgages-real-estate/08/house-flip.asp

Dave Ramsey Team. (n.d.). What is iBuying? Ramsey Solutions. Retrieved October 4, 2023, from https://www.ramseysolutions.com/real-estate/what-is-ibuying

Barber, G. (n.d.). Zillow’s algorithm can’t account for tasteful renovation. Wired. Retrieved October 4, 2023, from https://www.wired.com/story/zillow-ibuyer-real-estate/ 