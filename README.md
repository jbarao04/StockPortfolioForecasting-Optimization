# Stock Portfolio Forecasting and Optimization on S&P500

Made by: 

- [Filipe Barros](https://github.com/filipeazuil)
- [Joao Barao](https://github.com/jbarao04)
- [Goncalo Arrobas](https://github.com/Garrobas)



### Project Overview
The stock market is highly volatile, making it challenging to predict returns and optimize portfolios. This project aimed to leverage machine learning techniques to forecast daily returns of S&P 500 stocks and apply optimization methods to maximize cumulative returns. 

To streamline the process, we started with the SPY ETF, which mirrors the S&P 500. This allowed us to efficiently test and refine our models.

### Data
The dataset consists of daily trading data for all companies in the S&P 500, providing a comprehensive view of the index’s performance. This data includes essential features such as open, high, low, close prices, and trading volume for each stock.

- Training/Validation Period: 2010 to 2023
- Testing Period: January 2024

# Project Roadmap
### 1. Feature Engineering
Notebook: FeatureEngineering - SPYForecasting.ipynb

Description: Our approach involved creating a comprehensive set of features, including:

- Daily Trading Data (e.g., closing price, volume),
- Seasonality (Day of the week, month)
- Technical Indicators (like moving averages, MACD, Bollinger Bands),
- Macroeconomic Variables (interest rates, unemployment),
- Market Conditions inspired by Wyckoff’s methodology.

### 2. Model Testing and Strategy Development
Notebook: ModelTesting - SPYForecasting.ipynb

Description:
In this phase, we conducted extensive testing to refine our predictive strategy and identify the best configuration for the model. Our goal was to systematically evaluate the critical elements influencing model performance, ensuring optimal results. Key areas of experimentation included:

- Output Definition:
We tested whether predicting daily returns or closing prices yielded better performance. Ultimately, we opted for returns due to their stationary nature, leading to improved model stability and generalizability.

- Normalization:
Initial tests involved MinMax and Z-Score normalization for input features. However, normalization disrupted the relative relationships between features like moving averages and closing prices. After careful evaluation, we decided to proceed without normalization to preserve feature integrity.

- Time Step Analysis:
We experimented with different time step lengths (2, 5, 10, 20, 40) to determine the optimal window for capturing market patterns. Testing revealed that 5 time steps offered the best balance, improving both cumulative returns and directional accuracy.

- Loss Function Optimization:
To address issues of volatility mismatch, we explored various loss functions, including Mean Squared Error (MSE), Mean Absolute Error (MAE), and custom loss functions incorporating standard deviation penalties. Although the custom loss improved volatility alignment, it increased overall error, leading to MSE being selected for the final model.

### 3. Model Implementation and Evaluation
Notebook: ModelImplementation - SPYForecasting.ipynb and StockMarketPrediction.ipynb

Description:
This stage involved implementing a diverse range of machine learning models to predict daily returns and analyze market behavior. By comparing different architectures, we ensured our final model was not only accurate but also interpretable and efficient.

Models Implemented:

- Long Short-Term Memory (LSTM):

  - A unidirectional LSTM was chosen as the primary model for capturing sequential patterns in financial data.
  - Extensive testing showed that a single-layer LSTM with 32 units outperformed more complex multi-layer configurations.
  - Bidirectional LSTM was also tested, but the performance difference was negligible, leading us to prioritize the simpler unidirectional approach.

- Random Forest (RF):

  - Implemented as a baseline model to compare against LSTM. The RF model demonstrated strong performance in non-sequential tasks, providing valuable insights into feature importance.
  - GridSearchCV was used to tune hyperparameters such as the number of estimators, tree depth, and minimum samples per leaf.

- Support Vector Machine (SVM):

  - The SVM model was employed to test linear and non-linear relationships in the data. It performed well in capturing short-term patterns but lacked the sequential memory of LSTM.

- Decision Tree (DT):

  - As a simple baseline model, the decision tree offered fast predictions and interpretability, making it useful for feature analysis but limited in accuracy compared to ensemble and deep learning models.

### 4. Clustering and Unsupervised Learning
Notebook: Clustering - Data_Engineering.ipynb

Description:
To enhance model performance and tailor predictions to different types of stocks, we implemented an unsupervised clustering approach. This allowed us to group S&P 500 companies into five distinct clusters based on key financial characteristics:

- Clustering Criteria:

  - Volatility – Measured through standard deviation of daily returns.
  - Returns – Total returns for the 13 years

- Methodology:

  - We applied K-Means clustering to the dataset, leveraging the volatility and return metrics.
  - Companies were grouped into five clusters representing different market behaviors (e.g., high-volatility growth stocks vs. stable low-volatility blue chips).
  - This clustering ensured that companies with similar risk-return profiles were trained and modeled together, allowing the LSTM to capture more relevant patterns within each group.

- Benefits of Clustering:

  - Model Specialization: Each cluster had its own dedicated LSTM, optimizing performance based on the specific characteristics of that group.
  - Reduced Noise: By segmenting companies, the models could focus on more homogeneous data, improving pattern recognition and reducing overfitting.
  - Interpretability: Cluster analysis provided valuable insights into market segments, facilitating better understanding of sector behavior and risk dynamics.


### 5. Portfolio Optimization
Notebook: PortfolioOptimization - StockMarketPrediction.ipynb

Description:
Following the predictions generated by our models, we implemented portfolio optimization techniques to allocate weights across the S&P 500 companies. This process aimed to maximize returns while managing risk, aligning with different investor profiles.

- Optimization Techniques:

  - Monte Carlo Simulations – Simulated thousands of random portfolio allocations to identify the combination with the highest Sharpe ratio.
  - Genetic Algorithms – Applied evolutionary strategies to optimize portfolio weights, mimicking natural selection to converge on the most efficient allocation.

- Investor Profiles:
We designed three distinct investor profiles to reflect varying levels of risk tolerance, influencing how the optimization process weighed return and volatility:

  - Risk Averse – Prioritized minimizing volatility, favoring stable stocks with lower returns.
  - Moderate – Balanced approach between returns and volatility.
  - Greedy – Focused on maximizing returns, even if it meant higher exposure to volatility.

- Daily Weight Adjustments:

  - Portfolio weights were dynamically recalculated daily based on the updated predictions from our models, ensuring the strategy adapted to market changes.
  - This approach allowed for greater flexibility and responsiveness, enhancing the ability to capture short-term opportunities.
 
### 6. Results and Conclusions

Despite performing well on the training and validation sets, our model struggled on the test set (January 2024), ending the month with a loss. While the model successfully captured past patterns, it lacked robustness against new market conditions and sudden shifts, which highlights the unpredictability of stock markets.

# Requirements

To install the requirements, run:

```bash
pip install -r requirements.txt
```

# References

Books:

- Jacinta Chan (Year). Automation of Trading Machines for Traders.

- Rolf Schlotmann and Moritz Czubatinski (Year). Technical Analysis Masterclass.

- Rubén Villahermosa (Year). The Wyckoff Methodology in Depth.

Articles:

- Lim, M., & Zohren, S. (2021). Time-Series Forecasting with Deep Learning: A Survey. Highlighted the integration of deep learning models in financial forecasting, providing a broad perspective on state-of-the-art methodologies.

- Wei Bao, Jun Yue, and Yulei Rao. A deep learning framework for financial time series using stacked autoencoders and long-short term memory. PloS one, 12(7):e0180944, 2017

- Jigar Patel, Sahil Shah, Priyank Thakkar, and Ketan Kotecha. Predicting stock and stock price index movement using trend deterministic data preparation and machine learning techniques. Expert systems with applications, 42(1):259–268, 2015

- Chi-Ming Lin and Mitsuo Gen. Multi-criteria human resource allocation for solving multistage combinatorial optimization problems using multiobjective hybrid genetic algorithm. Expert Systems with Applications, 34(4):2480–2490, 2008.

- Brockwell, P. J., & Davis, R. A. (2002). Introduction to Time Series and Forecasting (2nd ed.). Springer. Served as a foundational text for understanding time series methods and their applications in financial forecasting.

- Glasserman, P. (2003). Monte Carlo Methods in Financial Engineering. Springer. Provided a detailed exploration of Monte Carlo methods in finance, enhancing risk management and pricing models.

