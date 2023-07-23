<div align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/banner.png" alt="Intro Logo" width="90%"/>
</div>

<h4 align="center">Forecasting Scrap Tier Receipt Rate at Multiple ATD Centers a Week into the Future using FB-Prophet</h4>

<hr>

## Table of contents
* [Introduction](#introduction)
* [About Dataset](#about-dataset)
* [Methodology](#methodology)
  * [EDA](#eda)
  * [Model Building](#model-building)
* [Results](#results)
* [Conclusion](#conclusion)
* [Acknowledgement](#acknowledgement)

## Introduction
American Tire Distributors (ATD) is one of the largest independent suppliers of tires to the replacement tire market. They operate more than 140 distribution centers, including 25 distribution centers in Canada, serving approximately 80,000 customers across the U.S. and Canada. Despite ATD being a leader in the industry, delivering over 44MM tires annually, ATD faces challenges how many scrap tires will be collected by a given Distribution Center every day. Answering this question can help them refine their  downstream logsitics of scrap tire recycling and reduce cost.

Under their sponsorhsip a hackathon was organized where a challenge was given to forecast the number of scrap tires that a ATD distribution center will receive a week ahead from a day.

## About Dataset
The dataset comprised of columns such as date, number of tires recieved, location of a ATD distrbution center, its zip code, size code of a tire and retail price of a scrap tire. Number of tires received i.e. total_tires variable in this project is the dependent variable that needs to be predicted. Historical data from September 20, 2020, to September 19, 2022 was available for analysis. The dataset can be found [here](https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/dataset/ATD_train_data.csv).

## Methodology
Before performing any forecasting, needless to say any Data Sciencey work, the most important part is to understand the trend and patterns in the data by performing exploratory data analysis (EDA). Since the task was to predict ahead in future, it was important to understand what are the factors affects the trend in the dependendent variable i.e. total_tires at a given day. 

### EDA
In the given data we notices there are four different distrbution centers provided. The following boxplots shows that the total tires received at different centers considerably vary. Thus it provided a hunch that for different location we have to build different forecasting model will be needed.

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig1.png" width="70%"/>
</p>

The size_code field in the data has particular importance as retail_price of the scrap_tires was different for different tire sizes which is pretty obvious. A simple correltion test performed on a particular location (Sacramento in the following right-image) for all different tire sizes provided moderate to significat correlation between number of tires received with retail price. Also By with plotly, by zooming on a particular period we realize that the retail price can vary on a particular day and can have different minimum maximum values across regions (left-image). This confirms that for **every different region, we will need a different forecast model** which will take into account the **retail price as a regressor**. 

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig8.png" width="350px" height="250px"/>
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig2.png" width="350px" height="250px"/>
</p>

Further analysis of the time-series data revealed that there are some weekly seasonal patterns in the data. The following right-image depicts that every sunday the dependent variable i.e. total_tires drops along with holidays such as Thaksgving, Labor-Day etc. This indicates that in our forecasting model other than retail_price, we would **need to include regressors such as IsSunday and Holiday**. Although while a 3d-Plot (left-image) was inspected for the effect of the day-of-the-week for different tire sizes, at a particular location, it was noticed that not only sunday but there is a **seasonal pattern in which Scrap tires are recevied more in the mid of the week, and falls off during weekend that is Fridays and Saturdays**. Thus it was finally decided to **include the Day-of-The-Week as regressor in the forecasting model**. Although the image is shown only for Sacramento but similar patterns were observed for other locations as well.

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig3.png" width="450px" height="250px"/>
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig5.png" width="450px" height="290px"/>
</p>

In the following box plot, the analysis from the last parapgraph can be observed as it can be seen that the number of scrap tires received at different locations vary slightly within the range of the 1st-3rd quartile. But a notable variance is there at the upper whisker especially for BaskerField. Also as mentioned earlier, for Sunday, there is little to no data.

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig4.png" width="60%"/>
</p>

### Model Building
With all this information gathered finally the model was decided to be build for different locations and different tire sizes. Retail Price, Holidays and Day-of-the-Week was added as a regressor for the dependent variable total_tires. A loop was ran for all the different combination of location and tire sizes. As data was available till September 19th, 2022, therefore the model was trained on data upto 09/12/2022 and out-of-sample test data was kept from September 13th to 19th. This replicates the requirement of predicting one week ahead in future. 

## Results
The following image was created by taking randomly 4 cases of location and tire size code to demonstrate the model performance. The image contains data only of the last month for better visibility. **The blue line is the actual with orange line is the predicted data**. As per the image it can be said that the model performed not bad.

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig6.png" width="60%"/>
</p>

The following graph visulaizes all the hold-out data together i.e. predictions made for all time serieses together against actual ground trurth data in one single graph. This is to confirm no anomalies present in any cases of location/tire-size. Blue line is the actual where organe is predicted data. As mentioned earlier, it can be said that the model has not done bad in predicting trend over the next week. But the model can definitely do better. 

<p align="center">
<img src="https://github.com/skswar/Forecasting_ScrapTire_Receipt_ATD/blob/main/img/fig7.png" width="60%"/>
</p>

**MAPE** or Mean Absolute Percentage Error was used as an evaluation metric to quantify model performance. MAPE is widely used for time series model evaluation. The issue of infinity was handled in the code. The MAPE result of the model **34%**.

## Conclusion
In this project, we applied time series analysis and forecasting using the FB-Prophet model, an industry-acclaimed tool praised for its robust algorithms, user-friendly configurations, and rapid processing capabilities. Time series analysis and forecasting serve as pivotal mechanisms that drive industrial growth, empowering businesses to make forward-thinking decisions underpinned by quantifiable future projections, rather than mere hunch/intuition.

This model's performace could potentially be enhanced through the incorporation of external data (e.g. weather) or the addition of other regressors into the algorithm that can either be dervied (e.g. **variance in retail price in the last 30 days**) or be gathered from ATD (e.g. **time of sales, promotion/discount season details, competitor's prices** etc.). If there are any suggestions or you are interested in a discussion regarding this project or any other Data Science related concepts, please feel free contact me through [Linked-In](http://linkedin.com/in/sayankrswar) or shoot an [email](mailto:sayankrswar@hotmail.com). I thank you for reading the entire article.

## Acknowledgement
I would like to thank the [American Tire Distrbutors (ATD)](https://www.atd.com/en/) and [Torqata](https://torqata.com/) for organizing the hackathon event and giving the opportunity to work on this open ended project and allowing to present my findings.

