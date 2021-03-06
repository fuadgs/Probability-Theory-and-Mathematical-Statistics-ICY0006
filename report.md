## New York Stock Exchange S&P 500 companies historical prices with fundamental data Dataset analysis 

### Stage 1

New York Stock Exchange  Kaggle Dataset is S&P 500 companies historical prices with fundamental data.


# Dataset consists of following files:

1. prices.csv: raw, as-is daily prices. Most of data spans from 2010 to the end 2016, for companies new on stock market date range is shorter. There have been approx. 140 stock splits in that time, this set doesn't account for that.
2. prices-split-adjusted.csv: same as prices, but there have been added adjustments for splits.
3. securities.csv: general description of each company with division on sectors
4. fundamentals.csv: metrics extracted from annual SEC 10K fillings (2012-2016), should be enough to derive most of popular fundamental indicators.

In addition, Prices were fetched from Yahoo Finance, fundamentals are from Nasdaq Financials, extended by some fields from EDGAR SEC databases.

# Following algorithms and actions could be applied by using this Data set:
- One day ahead prediction: Rolling Linear Regression, ARIMA, Neural Networks, LSTM
- Momentum/Mean-Reversion Strategies
- Security clustering, portfolio construction/hedging

Datasets consist of following variables:

- Open Price is a price of stock when stock market is opened.
- Close Price is a price of stock after closure stock market.
- Low Price is a minimal price of stock during the day. 
- High Price is a highest price of the stock.
- Volume is measured in the number of shares traded.
- Ticker Symbol is a Symbol of Stock in New York Stock Exchange market.
- Accounts Payable is money owed by a business to its suppliers shown as a liability on a company's balance sheet.
- Accounts receivable are legally enforceable claims for payment held by a business for goods supplied or services rendered that customers have ordered but not paid for.
- Return on equity (ROE) is a measure of financial performance calculated by dividing net income by shareholders' equity.
- Capital expenditure or capital expense is the money an organization or corporate entity spends to buy,
- Capital surplus, also called share premium, is an account which may appear on a corporation's balance sheet, as a component of shareholders' equity, which represents the amount the corporation raises on the issue of shares in excess of their par value of the shares.
- The cash ratio is a measurement of a company's liquidity, specifically the ratio of a company's total cash and cash equivalents to its current liabilities.


### Stage 2

In order to make stock price prediction Open Price, Close Price, Low Price and Volume variables are used. 
In general following could be told based on figure below:

![Median, Mean, Minimal, Standard Deviation values](Images/777.png)

As it is visible from table above the Mean openining is 70.83 whereas mean closing value is 70.85. In addition, the mean low and high values are 70.11 and 71.54 respectively. 
The maximum mean values are fluctiating between 1549 and 1600.





### Stage 3

The highest correlations are observed between Volume and Low Price,  as well as between Close Price and Volume. It make sense because the lower prices are the more investers get interested in investing and also the less the plosing price is the more "buy" operations performed on stock next day. 

Five independent variables - namely,  Open Price, Close Price, Low Price and Volume are used in the scatter plots. 

Scatter and Density plot is constructed showing the relationship between each other of Open Price, Close Price, Low Price and Volume variables

![Scatter and Density plot](Images/result2.png)

Also, by using RM, LSTAT, and PTRATIO the linear regression model has been implemented and scatter plots with line have been developed.

You can see the generated plot of stock price prediction by using Linear regression below:


![Linear Regression](Images/stockpriceslinearregression.png)


In addition, analysis is done on Facebook and Microsoft stocks too. 
<br>

![MSFT](Images/msft.png) 

![FB](Images/fb.png)


### Stage 5


The regression models using training set and testing set by using the following functions below:

```
def plotter(code):
    global closing_stock
    global opening_stock
    f, axs = plt.subplots(2,2,figsize=(8,8))
    plt.subplot(212)
    company = df[df['symbol']==code]
    company = company.open.values.astype('float32')
    company = company.reshape(-1, 1)
    opening_stock = company
    plt.grid(True)
    plt.xlabel('Time')
    plt.ylabel(code + " open stock prices")
    plt.title('prices Vs Time')
    plt.plot(company , 'g')
    
    plt.subplot(211)
    company_close = df[df['symbol']==code]
    company_close = company_close.close.values.astype('float32')
    company_close = company_close.reshape(-1, 1)
    closing_stock = company_close
    plt.xlabel('Time')
    plt.ylabel(code + " close stock prices")
    plt.title('prices Vs Time')
    plt.grid(True)
    plt.plot(company_close , 'b')
    plt.show()
for i in comp_plot:
    plotter(i)
```


```
    def process_data(data , n_features):
    dataX, dataY = [], []
    for i in range(len(data)-n_features-1):
        a = data[i:(i+n_features), 0]
        dataX.append(a)
        dataY.append(data[i + n_features, 0])
    return np.array(dataX), np.array(dataY)
 ```   
 ``` 
    def model_score(model, X_train, y_train, X_test, y_test):
    trainScore = model.evaluate(X_train, y_train, verbose=0)
    print('Train Score: %.5f MSE (%.2f RMSE)' % (trainScore[0], math.sqrt(trainScore[0])))
    testScore = model.evaluate(X_test, y_test, verbose=0)
    print('Test Score: %.5f MSE (%.2f RMSE)' % (testScore[0], math.sqrt(testScore[0])))
    return trainScore[0], testScore[0]
```


The linear model performance for training set and test set are below:

![Performance](Images/RMS.png)

In addition, the accuracy of the model is measured and visible in graph below where
Red - Predicted Stock Prices  ,  Blue - Actual Stock Prices

![Performance](Images/acc.png)
