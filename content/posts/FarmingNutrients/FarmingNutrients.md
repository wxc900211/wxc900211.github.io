---
title: "Geographic Visualization of Nutrients from Crops"
subtitle: ""
date: 2023-10-22T21:06:12+02:00
lastmod: 2023-10-22T21:06:12+02:00
draft: false
author: "Xiaochen"
authorLink: "/about/"
description: "Geographic Visualization of Nutrients from Crops"
license: ""
images: []

tags: ["Visualization","Agriculture"]
categories: ["Data Analysis"]

featuredImagePreview: "https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/FarimingPreview.png"
featuredImage: "https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/Farming.png"

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
This is a **fertilizer** company and our main assigment is to sell fertilizers to the farmers. We are updating our products based on the data get from farmers.

We collect samples from farmers' fields around the world and then send the samples to the labs. The samples, which are from **soil**, **water**, **leaves** etc., are analyzed with different ways.

We observe the results of the samples' elements, like **nitrogen**, **phosphorus**, and **potassium** - three primary nutrients plants need to grow and vusualize the trend on the map through longtitude and latitude. 

{{< admonition tip "Understand the Primary Nutrients in Fertilizer" >}}
**Nitrogen (N)** plays a key role in a plant's **coloring and chlorophyll production**, making it an important factor in leaf development. 

**Phosphorus (P)** plays a key role in the **growth** of roots, blooming, and fruiting.

**Potassium (K)** contributes to the overall **health and vigor** of plants.
{{< /admonition >}}

**100,727,139**results are recorded by now.

Here, we take one country as an example to show the final effect.

## 2 Data Preparation

* **Data Cleaning**

The data is recorded mannually, so we unify the name of elements and units of analysis results.

{{< admonition example "Examples" >}}
**The name of elements** :*Phosphorus*

Phosphorus Phosphate, P2O5 Avial, P Grain, Tot P, Total P, P(SZ), P Tot, P1, P2

**The units of analysis results** : *ppm*

1 ppm = 1 mg/kg =1 ug/g ≈ 1mg/l
1 kg/ha =2.24 ppm

{{< /admonition >}}

* **Data Calculation** 

1. **Actual Results:** 2 columns of values are related to the final result: Decimal Result and Decimal Places.

{{< admonition example "Examples" >}}

Decimal Result = 2

Decimal Places=1

**Actual Result= 2/10^(1)=0.2**

{{< /admonition >}}

2. **Group results :** The minimum area unit showed is **PostCodeArea**, so we average the value in each PostCodeArea by year. In the end, we have 121*21=2541 rows of data prepared for visulazation.


## 3 Geographic Visualization

We choose **"choropleth map"** and use **Plotly Dash** through Python. A **geojson** file of the country is necessary.

{{< admonition tip "Understand the choropleth map" >}}

It’s made by separating the area being mapped, such as by geographic or political boundaries, and then filling each resulting section with a different color or shade. Each color or shade represents a different variable, and/or a different value or range for a single variable. 

This makes choropleth maps useful for visualizing clusters of data across a **geographic area** while maintaining the context of **regional boundaries**.
{{< /admonition >}}

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/v1.gif)

This is an **interactive** visualization and the code is available for deployment.

See how the values change with time (PS: The country and data displayed on this website is faked) :

1. Choose **Element** and **Year**.
2. **Zoom in** or **zoom out** to check the details.

(PS: The country and data displayed on this website is faked.)

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/f1.png)

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/f2.png)

## 4 Business Value

We regard this as a part of our product. Not only sell the product, but also provide essential **service** to farmers.Usually the unit price of our products is higher than other companies', however, we want to guide farmers how to use fertilizers more efficiently. 













