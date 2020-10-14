# **Perfecting Portfolios: Optimization and the Efficient Frontier**
*A Python multi-level portfolio optimization analysis by Juan Cajigas, Mindy Bright and Steffen Westerburger* 

![Optimization](https://www.financemagnates.com/wp-content/uploads/2019/03/97198047_Traders_work_on_the_floor_of_the_New_York_Stock_Exchange_NYSE_in_New_York_US_on_Friday-xlarge_trans_NvBQzQNjv4BqgsaO8O78rhmZrDxTlQBjdEbgHFEZVI1Pljic_pW9c90-Edited.jpg)

## **Optimization: The Art of Making the Best of Anything** 

In this project we use the Python skills we learned in the course so far and apply these to the financial concept of stock portfolio optimization. By using these technical skills tied to a fundamental topic in financial analysis we aim to showcase the strengths and opportunities of FinTech.

The main research question we try to answer is how we can determine the optimal weight distribution of individual stocks within any given portfolio. This is important for several reasons:

- A balanced portfolio provides a greater likelihood for someone to reach their financial goals;
- It democratizes portfolios by enabling investors to pick and choose their preferred stocks;
- It acknowledges that every investor is different and holds different risk profiles.

It is no secret that a lot of research is done on this subject of portfolio management and optimization. For the sake of time we decided to limit the scope of this project to three main investment optimization theories. We do however realize a more multifaceted analysis is necessary to apply our tools in the real world. 

The cornerstone of our project will use three financial performance indicators:

1. [The Sharpe Ratio](https://www.investopedia.com/terms/s/sharperatio.asp)
2. [The Sortino Ratio](https://www.investopedia.com/terms/s/sortinoratio.asp)
3. [The Generalized Sharpe Ratio](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.523.5419&rep=rep1&type=pdf)


 Our optimization efforts will focus on these slightly different criteria to give us a better insight in potential risk and return on investment of our handpicked and personalized portfolios. Subsequently we will plot these simulated optimizations and create an ['Efficient Frontier'](https://www.investopedia.com/terms/e/efficientfrontier.asp) which will give us a better understanding of the relation between portfolio volatility and return. 

We will also demonstrate the effects of allowing for short selling in the portfolio selection process. When the investor has the choice to allocate negative weights in some of the assets, the expected return of the portfolio can, potentially, increase substantially without altering too much the level of risk. 
 
 To further show the potential value of these optimization efforts, we will also create a ['Monte Carlo Simulation'](https://www.investopedia.com/terms/m/montecarlosimulation.asp) which is a multiple probability simulation that gives us insight in potential returns of these portoflios over time. This can be a particularly handy tool for any investor to plan for important milestones/investment strategy.



## **A Closer Look: Stock Data and the Alpaca API** 

In order to ensure reliability and validity of our analyses, it is important to focus on the data we use to run our analyses. The backbone of the financial data we use in this project is provided by the [Alpaca API](https://alpaca.markets/algotrading?gclid=EAIaIQobChMIlJu5-_ux6QIVGW-GCh2XRAO9EAAYASAAEgKSHfD_BwE). The use of this API enables us to pull in real-time stock information of all tradeable stocks. 

The information we are interested in is primarily the close price of every tradeable stock per trade day. This raw data gives us the perfect start for our analysis. Another benefit of using the Alpaca API is that the process of data cleaning is straightforward. By simply dropping the columns in our dataframe that we will not be using, a 'clean' and handy dataframe for us to work with is what remains. 

In our interface the investor has the option to select directly the candidate stocks for her portfolio. For this demonstration we have selected the following:

    . American Airlines (AA)
    . Apple (AAPL)
    . Citibank (C)
    . Facebook (FB)
    . Goldman Sachs (GS)
    . Johnson and Johnson (JNJ)
    . JP Morgan Chase (JPM)
    . Morgan Stanley (MS)
    . Procter and Gamble (PG)
    . Tesla (TSLA)

We have selected historical data (close prices) covering the year 2019.

![statistics](png/describe_data.PNG)


This is the plotting of returns for the 10 stocks:


![returns](png/return_stocks.png)
### *Challenges we ran into during the data exploration and clean up process*

Irrespective of the straightforward process of data cleaning, we did face some important data-related questions we needed to answer. Optimization and portfolio analysis heavily relies on accuracy of the underlying data used for these analyses. In other words, it is very important to realize the limitations and characteristics of the pulled in stock data, and more importantly of the covered time span. If, for example, the data only covers a time span in which the overall stock market was over-performing, the outcome of the analysis will be skewed/show overly optimistic outcomes as well. The same is true if the dataset primarily covers a period of economic downturn. It is important to realize the existence of these potential limitations this when interpreting result of our analyses. 


## **Data Analysis: Sharpe, Sortino, Efficient Frontier and Monte Carlo** 

In this section we will present the framework of our analyses and discuss interesting challenges and outcomes. In order to better understand the code we developed and how it can be used, we would like to walk you through the fictional scenario of investor X. 

The first question investor X will be asked, is to pick the number of stocks he/she would like the portfolio to consist of. After that the user will be asked to specify what specific stocks should be included. The tickers of these stocks or subsequently used as in input variable for our Alpaca API resulting in the dataframe of the handpicked stocks. Also there is a possibility to specify start and end date of the data that will be used.



### **The Sharpe Ratio**

The Sharpe Ratio uses annualized cumulative stock returns and the standard deviation of the annualized cumulative stock returns. Basically, it determines the average return earned in excess of the risk-free rate per unit of volatility. For example, the yield for a U.S. Treasury Bond could used as a risk-free rate. The higher the value of the Sharpe-ratio, the greater the risk-adjusted return is. 

This is a  scatter plotly_express diagram of 100,000 portfolios. The weights were randomly  generated by a continuous uniform distribution: 

![scatter_sharpe](png/scatter_sharp.png)

We can see how the Sharpe ratio reaches a value of around 3.0 for volatilities in the range  15% - 20% and returns of around 50% (annualized).

Finding the optimum Sharpe radio by sampling would required a very high number of drawings. Specially when the number of assets is 10 as it is in our case. Instead we use a optimization routine where the best weights combination can be found very quickly. A description of the optimization routine we used can be found in a section below in this document. 
Using the Sharpe ratio this is the optimum weights combination we found for our choice of stocks:


![pie_sharpe](png/Pie_Sharpe.png)

#### **Efficient Frontier**

The above mentioned ratios can be used in our optimization efforts when we use the results to create the efficient frontier. The efficient frontier is an analysis and representation of optimal internal portfolio weights that would offer the highest expected return for any defined level of risk (Investopedia) These outcomes are dependent on combinations of weights that make up the portfolio. When we use the Sharpe ratio this is the frontier we found:

![EF_sharpe](png/EF_Sharp.PNG)

The red line specifies the maximum Sharpe ratio that can be achieved for a specific level of volatility. The red dot is the optimal Sharpe ratio. In this case it was 3.74 when the volatility of the portfolio is  15.48% and the return 57.9%.

### **The Sortino Ratio**

The Sortino Ratio is a different version of the Sharpe Ratio. Instead of calculating the standard deviation of a stock or portfolio, it solely focuses on the asset's standard deviation of the negative returns. In other words it is only looking at downside deviation, instead of the total standard deviation (Investopedia) This is a very welcome adjustment of the 'regular' Sharpe-ratio, because investors would generally welcome any upside volatility. The Sortino ratio alleviates punishment for good (upward) risk which is neglected in the Sharpe-ratio.

We recalculated the standard volatility of the portfolio by filtering out positive returns, and then ran our optimization routine. In this particular case the optimum is achieved when 100% of funds are allocated to AAPL. It looks like in 2019 Apple was indeed an amazing investment! The dotted blue line in the graph below represents the Sortino based efficient frontier:


![EF_Sortino](png/EF_Sharp_Sortino.PNG)

The blue dot is the optimum Sortino ratio that coincides with the volatility and return of AAPL for this period: vol of 26.2% and return of 86.5%.


### **The Generalized Sharp Ratio**
The Sharpe ratio uses only the expected return and volatility of the returns of assets. Investors can also be interested in picking a portfolio with positive skew, that is, a portfolio with a long right hand tail in the distribution. 

![skew](png/Negative_and_positive_skew_diagrams_(English).svg.png)https://en.wikipedia.org/wiki/Skewness



We modify the Sharp ratio by proportionally scaling it with the the skew of returns. A positive skew can be a desired feature as the average return across all the stocks would be greater than the median return. 

![skew_stocks](png/Positive_skew.png.jpg)Sourced from https://www.indexologyblog.com/2017/04/28/skewered/


This is the optimal mix of weights we found using the Generalized Sharpe ratio in our optimization routine:

![Pie_SharpeG](png/Pie_SharpeG.png)

Using this metric the prefer stock is PG, followed by AAPL and FB. We noticed how JPM is dropped from the list (comparing to ordinary Sharpe ratio).

The efficient frontier is the dotted green line in the graph below. We can compare it to the Sharpe ratio (red) and Sortino ratio (blue):


![EF_SharpeG](png/EF_Sharp_sortino_SG.PNG)

### **Allowing for short-selling**
To make the portfolio more realistic, we allow for individual short selling on only 10% of the total value of the portfolio. We do this in our algorithm by redefining the bounds of the parameters to [-0.10, 1.0]. As expected, we found negative allocation on stocks that were not selected running the ordinary Sharpe ratio based optimization.

![weights_ss](png/Weights_short_selling.png)


The efficient frontier for the new portfolio is depicted by the continuos green line below. By allowing the optimizer to select negative weights for the underperforming stocks the expected return can be increased by a considerable amount. 

![EF_SS](png/EF_Sharp_SS.PNG)

## **Optimization Routine**
The three ratios we are analyzing are proportional to the expected rate of return and inverse to the volatility of the portfolio. Our goal therefore is to maximize the ratio. To this aim we employ the SciPy python library (https://www.scipy.org/). This is a very powerful scientific computing library that includes modules not only for optimization but also for linear algebra, integration, interpolation, special functions, FFT, signal and image processing, ODE solvers and other tasks common in science and engineering (Wikipedia). From the *optimize* module (https://docs.scipy.org/doc/scipy/reference/tutorial/optimize.html) we selected the Sequential Least SQuares Programming Algorithm (SLSQP). This is the right algorithm for our problem as the objective function (the ratio) is multivariate scalar and the optimization requires one or multiple constraints (portfolio weights and volatility points for the efficient frontier). The routine finds the minimum of the objective so we specified the function as the negative of the ratio. Once the optimum is found we just have to change the sign to obtain the maximum.   

We minimize the negative of the ratio subject to the condition that the sum of weights equals one. If we don't want to allow for short selling the bounds of the parameters (weights) are defined by the set [0,1]. If short selling is allowed then the left-handed bound can be adjusted as required. Each point of the efficient frontier is the maximum ratio subject to a specific volatility level. So for the construction of the frontier, we loop over a set of optimization routines where besides the sum of weights constraint we add another one for the volatility level.


### **Monte Carlo Simulation**

The Monte Carlo simulation is a multiple probability simulation that is often used for financial analysis. It used to model probability of different outcomes that would otherwise be difficult to predict because of the interference of uncertain variables such as return rates (Investopedia). This uncertainty is addressed by using a simulation of multiple different potential values for any variable (based on historic information) rather than just using a single value. 

![MC](png/MC.png)
![MC_histogram](png/MC_histogram.png)


## **Discussion of Findings**
- Mean-Variance portfolio selection schemes tend to concentrate portfolios in a few assets. When using the Sharpe ratio in our analysis only 4 out of 10 stocks were selected.
- Optimization routines from ScyPy are extremely fast finding a solution and are easy to use.
- The Sortino ratio produced surprising results by allocating 100% of funds to one single stock.
- Short selling set-ups are very easy to incorporate into the optimization routine. 
- For the MC set-up, as we were considering multiple assets, we structured the data in a dataframe. 
- The selection of the time window for the estimation of parameters is critical. Although different techniques used in the analysis affect the final result, we found that the selection of the historical data can also be very influential.

## **Postmortem**
- As the number on assets grows, the number of sampling data points increases exponentially. This is the reason why it is very difficult to achieve an optimum employing that set-up. Sampling, in this case, can provide some insight into the distribution of risk and returns, but ultimately an optimization algorithm is required to find the efficient frontier.
- Although the initial idea in our project was to set-up a nice user interface (user indicating what stocks to include in the analysis) we found this task difficult and very time consuming. We decided instead to incorporate a simple 'input' framework that was suitable for our purposes.
- We were not able to overlay two plotly graphs into a single one. 
- In the future we would like to extend the analysis in different aspects:

    - Build a more advanced interface for the user
    - Overlay plotly graphs
    - Add more portfolio selection criteria variables 
    - Use different time frameworks
    - Explore the use of more efficient data structures for the running of the Monte Carlo experiment. 























