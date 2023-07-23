<div align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/banner.png" alt="Intro Logo" width="90%"/>
</div>

<h4 align="center">Forecasting Scrap Tier Receipt Rate at Multiple ATD Centers a Week into the Future using FB-Prophet</h4>

<hr>

## Table of contents
* [Introduction](#introduction)
* [About Dataset](#about-dataset)
* [Methodology](#methodology-and-results)
  * [EDA](#eda)
  * [Model Building](#model-building)
* [Results](#results)
* [Conclusion](#conclusion)
* [Acknowledgement](#acknowledgement)

## Introduction
American Tire Distributors (ATD) is one of the largest independent suppliers of tires to the replacement tire market. They operate more than 140 distribution centers, including 25 distribution centers in Canada, serving approximately 80,000 customers across the U.S. and Canada. Despite ATD being a leader in the industry, delivering over 44MM tires annually, ATD faces challenges how many scrap tires will be collected by a given Distribution Center every day. Answering this question can help them refine their  downstream logsitics of scrap tire recycling and reduce cost.

Under their sponsorhsip a hackathon was organized where a challenge was given to forecast the number of scrap tires that a ATD distribution center will receive a week ahead from a day.

## About Dataset
The dataset comprised of columns such as date, number of tires recieved, location of a ATD distrbution center, its zip code, size code of a tire and retail price of a scrap tire. Historical data from September 20, 2020, to September 19, 2022 was available for analysis. The dataset can be found [here](https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/dataset/ATD_train_data.csv).

## Methodology
Before performing any forecasting, needless to say any Data Sciencey work, the most important part is to understand the trend and patterns in the data by performing exploratory data analysis (EDA). Since the task was to predict ahead in future, it was important to understand what are the factors affects the trend in the dependendent variable i.e. total_tires at a given day. 

In the given data we notices there are four different distrbution centers provided. The following boxplots shows that the total tires received at different centers considerably vary. Thus it provided a hunch that for different location we have to build different forecasting model will be needed.

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig1.png" width="70%"/>
</p>

The size_code field in the data has particular importance as retail_price of the scrap_tires was different for different tire sizes which is pretty obvious. A simple correltion test performed on a particular location (Sacramento in the following right-image) for all different tire sizes provided moderate to significat correlation between number of tires received with retail price. Also By with plotly, by zooming on a particular period we realize that the retail price can vary on a particular day and can have different minimum maximum values across regions (left-image). This confirms that for **every different region, we will need a different forecast model** which will take into account the **retail price as a regressor**. 

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig8.png" width="350px" height="250px"/>
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig2.png" width="350px" height="250px"/>
</p>

Further analysis of the time-series data revealed that there are some weekly seasonal patterns in the data. The following right-image depicts that every sunday the dependent variable i.e. total_tires drops along with holidays such as Thaksgving, Labor-Day etc. This indicates that in our forecasting model other than retail_price, we would **need to include regressors such as IsSunday and Holiday**. Although while a 3d-Plot (left-image) was inspected for the effect of the day-of-the-week for different tire sizes, at a particular location, it was noticed that not only sunday but there is a **seasonal pattern in which Scrap tires are recevied more in the mid of the week, and falls off during weekend that is Fridays and Saturdays**. Thus it was finally decided to **include the DayofTheWeek as regressor in the forecasting model**. Although the image is shown only for Sacramento but similar patterns were observed for other locations as well.

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig3.png" width="450px" height="250px"/>
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig5.png" width="450px" height="250px"/>
</p>










