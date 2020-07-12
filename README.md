# What is ARIMA?
ARIMA is short for Autoregressive Integrated Moving Average that takes in three parameters:
- p : the number of time lags
- d : the degree of differencing
- q : the order of the moving average
An ARIMA model is usually made fit to historical time series data and used to forecast future points in the series. Common applications may involve product demand predictions, price predictions, or visitor estimations, sometimes with a seasonal trend.

## Source Code
[https://github.com/andy971022/SP500-ARIMA
](https://github.com/andy971022/SP500-ARIMA
)
## When to Use Arima?
- When you have a time series data.
- When your time series data is stationary.
- When you want to predict the near future.

## What Do You Mean by Stationary?
- [Detail](https://en.wikipedia.org/wiki/Stationary_process)
- In Short: When you data does not show a time-depending trend.

### Examples

The following images will show up again in our tutorial, but we'll use them to explain what a stationary process is. The first image down below is S&P500 ETF's(SPY) stock price, and the second image is the second differentiation of that stock price from August 2019 to November 2019. 

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/fc06d69e-c4b0-98b5-c4ee-e55c7c9460be.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/60aec030-54ee-5e6c-931d-fefcf056ccde.png)

The first image is not stationary because it has an obvious up-going trend correlating with time. The second image, however, shows a stationary process because the data points revolve around a constant value, meaning that they do not vary and correlate with time.

# Steps to Create An ARIMA Model
1. Collect a set time series data.
2. Obtain the parameter d : do an [ADF(augmented Dickey–Fuller test)](https://en.wikipedia.org/wiki/Augmented_Dickey%E2%80%93Fuller_test) test on the dataset and various orders of differentiation to obtain a stationary process.
3. Obtain the parameter p : Do an [Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation#:~:text=Autocorrelation%2C%20also%20known%20as%20serial,the%20time%20lag%20between%20them.) test on the dataset with respect to the selected order d. 
4. Obtain the parameter q : Do an [Partial Autocorrelation](https://en.wikipedia.org/wiki/Partial_autocorrelation_function#:~:text=In%20time%20series%20analysis%2C%20the,not%20control%20for%20other%20lags.) test on the dataset with respect to the selected order d. 
5. Create the ARIMA(p,d,q) model with the deduced parameters
6. Fit and forecast


# Step 1
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/2085dfcb-3d54-59ab-2266-a16e6262c33e.png)
We'll get the historical closing stock prices for S&P500 from `yfinance` and predict the stock prices for November 2019.
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/9804b368-8b3e-7933-1feb-117fc5086d5d.png)
We plotted the graphs and saw that the stock prices are far from being stationary.
So, let's cherry-pick just the data three months prior to our target of prediction.

# Step 2
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/be22485c-98c0-8ed1-6349-9e7fab2d65b7.png)
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/c9fdcb06-0560-8252-6409-1c9b01f99c42.png)
We'll create the first and second differences and compute the ADF tests following on.
The p-value of the first differentiation, -8.17674265403581, is less than -2.8853397507076006, the 5% alpha value, and even less than the 1% alpha value. That is, we can reject the null hypothesis, stating that the data is stationary.

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/40e8cf40-bd76-bd47-e459-28ab5ecdd7cd.png)
We see that the first difference is well enough for the ARIMA model. Therefore, we'll Set the parameter d to 1.

# Step 3
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/940a4932-45a3-1590-01c1-1973d278859d.png)
We have computed the Autocorrelation test, and the graph shows that 1 is a suitable value for p. Why? Because we need to truncate the first lag in order to have the rest of the autocorrelations fall within the 95% confidence interval.

# Step 4
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/1927271d-c3b7-81b2-b34a-7d326a9deea5.png)

With similar reasons, q is selected as one.

# Step 5
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/8d552f41-88da-e05e-9088-d360c39c977a.png)
Now we have deduced an ARIMA(1,1,1) model and will fit the closing stock prices into the model. Be careful not to throw in the first difference of the stock price into the model.

# Step 6
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/7b67b470-1f74-02e3-7e6e-034f18a11007.png)
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/503314/808fc723-01ba-7b95-021a-146ba07a15a9.png)

We have fed data three months prior to our prediction set into the model. The second last image is our prediction while the last one is the actual data. We see that our model is able to predict a similar trend.
