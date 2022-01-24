# **Subsequence Time Series Clustering**
![](https://i.ytimg.com/vi/wqQKFu41FIw/maxresdefault.jpg)
![](https://img.shields.io/github/license/huytjuh/Recommender-System-Basket-Analysis) ![](https://img.shields.io/maintenance/no/2019)

Subsequence Time Series (STS) Clustering model for discovering hidden patterns and complex seasonality within univariate time-series datasets by clustering similar groups of time windows based on their structural characteristics with advanced statistics.

Python implementation from scratch inspired by paper [Wang et al. (2006)](https://link.springer.com/content/pdf/10.1007/s10618-005-0039-x.pdf)

***Version: 2.1 (2021)*** 

---

## Introduction
Time-series analysis allows us to predict future values based on historical observed values, but they can only do so to the point where the model is able to differentiate between seasonal fluctuations within the univariate time-series dataset. So far, many papers consider relatively simple seasonal patterns such as weekly and monthly effects. However, higher frequency time-series often exhibit more complicated seasonal patterns. An alternative to using dummy variables, especially for multiple complex seasonal patterns, is to use Fourier terms. Using linear combinations of sine and cosine functions, successive Fourier terms represents the harmonics of the multiple seasonality components, and thus can be added as explanatory regressors to the forecasting models.

While traditional Fourier term analysis is able to capture the pattern of an univariate time-series relatively well, it tends to overestimate for day-of-the-week, monthly, and other known events that are self-evident without requiring extensive analysis. Hence, we suggest residual modified Fourier terms obtained from the residuals of ARIMA, allowing us to redirect our focus on capturing the more complex hidden patterns. On top of that, we propose a Subsequence Time series Clustering framework to enforce the forecasting models to adjust their parameters according to the clustered seasonal time windows, bringing the complex seasonality model to another level. That is, by incorporating advanced statistical operations and defining more complex characteristics of the univariate time-series such as non-linearity, self-similarity, and chaos on top of the decomposed time-series (trend, seasonality, noise), allows us to further refine and improve the forecasting accuracy using a fully data-driven framework.

## Colab Notebook

STS Clustering based on Hierarchical Clustering:<br/>
[Google Colab]() | [Code]()

STS Clustering based on Self-Organizing Maps (SOM):<br/>
[Google Colab]() | [Code]()

## Prerequisites
* Linux or macOS
* python 3.8
* nolds 0.5.2
* pmarima 1.8.4
* bayesian-optimization 1.2.0
* CPU or NVIDIA GPU + CUDA CuDNN

## Getting Started

### Installation
* Clone this repository.
```
git clone https://github.com/huytjuh/Subsequence-Time-Series-Clustering
cd Subsequence-Time-Series-Clustering
```
* Install Python dependencies using `requirements.txt`.
```
pip install -r requirements.txt
```

### Run Recommender System
* Download an univariate time-series dataset:
```
datasets/station_case.csv
```
* Train STS Clustering 
```
#!./scripts/run_train.sh
python3 train.py
```
* Test STS Clustering on ARIMA forecasting model
```
#!./scripts/run_main.sh
pyton3 main.py --model ARIMA
```

## Algorithms
The table below lists the global measures describing the univariate time-series obtained using advanced statistical operations that best capture the underlying characteristics of the given time horizon. The following global characteristics are measured and scaled normally: Trend, Seasonality, Periodicity, Serial Correlation, Skewness, Kurtosis, Non-Linearity, Self-Similarity, and Chaos. References and formulas are linked in the Reference column, explaining in detail the math and implementation of the statistics.


| Algorithms                                | Type                                   | Description                                                                                                                                                                                                                       | Code |
|-------------------------------------------|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------|
| Popularity<br /> (pop)                    | Naive                                  | Naive recommendations based on most popular items bought by users and not in the basket.                                                                                                                                          | [Code]() |
| Cosine<br /> (CF(cos))                    | Collaborative Filtering (Memory-based) | Cosine-based similarities calculated from the cosine of the angle between two items thought of as two vectors in the m dimensional user-space.                                                                                    | [Code]() |
| Conditional Probability<br /> (CF(cp))    | Collaborative Filtering (Memory-based) | Conditional probability based similarities taking rating scale between users into account and normalized including a control variable alpha to penalize popular items.                                                            | [Code]() |
| Bipartite Network<br /> (CF(bn))          | Collaborative Filtering (Memory-based) | Bipartite network based similarities calculated from a bipartite graph describing the shopping basket data containing two nodes: consumers and products; thus, can be defined as the transition probability between each product. | [Code]() |
| Alternate Least Square<br /> (ALS)        | Collaborative Filtering (Model-based)  | Matrix factorization algorithm for explicit or implicit feedback in large datasets by decomposing the user-matrix into smaller dimension user and item features.                                                                  | [Code]() |
| Factorization Machine<br /> (FM)          | Collaborative Filtering (Model-based)  | Extended matrix factorization model allowing for feature-rich datasets by including higher-order interactions between variables of larger domain and combining both regression and factorization methods.                         | [Code]() |
| Basket-Sensitive Random Walk<br /> (BSRW) | Hybrid                                 | A stochastic process dictating the likelihood of jumping from one item to another as extension to further explore transitive associations by incorporating the current shopping context into the Collaborative Filtering models.  | [Code]() |

## Test Results & Performances
A comparison between seasonal self-evident explanatory variables that fall under the naive methods and STS clustering methods that fall under the more complex methods. We run the evaluation on five different forecasting models, namely ARIMA, RF, LSTM, Hybrid ARIMA-RF, and Hybrid ARIMA-LSTM. Additionally, we provide a [Notebook]() to illustrate how the different algorithms could be evaluated and compared.

| <br /> Algorithm | L-3-O<br /> bHR(pop) | L-3-O<br /> bHR(rnd) | L-1-O<br /> wHR(loo) |
|---|:---:|:---:|:---:|
| pop | 0.43 | 16.80 | 2.83 |
| CF(cos) | 16.72 | 31.62 | 5.65 |
| CF(cp) | 16.46 | 30.84 | 5.67 |
| CF(bn) | 16.75 | 31.88 | 5.79 |
| CF(cos) + BSRW | 16.63 | 31.70 | 5.67 |
| CF(cp) + BSRW | 16.46 | 30.80 | 5.71 |
| CF(bn) + BSRW | 16.75 | 31.84 | 5.78 |
| ALS | 15.28 | 26.28 | 4.34 |
| BFSM* | 20.17 | 19.21 | 2.25 |
| Hybrid | 15.28 | 26.36 | 4.32 |

*A subset of 10% of the testing had to be taken instead due to its computational heavy nature of Factorization Machine


%Then, it is important to illustrate the added value of the explanatory variables an clustering methods. Do these additions actually improve forecasting performance? Keeping in mind that adding explanatory variables falls under the more simple methods, along with the random walk, and the clustering methods fall under the more complex methods. Once we have established that including both clustering and the explanatory variables is advantageous for predicting TV ratings, possibly by stating the average increase in RMSE or the fact that it increased in all cases, we can start actually discussing the results.
Investigating the possible added value of including the explanatory variables and clustering methods in improving the forecasting performance of the models, the results in Table \ref{tab: Added value} show that the baseline model is never the best variation for all of the models and any of the forecasting horizons. We had expected the explanatory variables to have added value, however this was not the case for the clustering method due to the ongoing debate in the recent literature. Even though, as can be seen in the table, there is no consensus on a single model variation for the models or the forecasting horizons, the majority of the models has the lowest average RMSE when including explanatory variables and clustering, where the remainder of models supports adding only the explanatory variables. However, as the support is far from unanimous and the differences are small, we have decided to not limit ourselves to using only this single model variation in the next step of preliminary results. More importantly, based on these results, on average we are able to beat the random walk for short term forecasting. 


## Reference Papers

* Li, M., Dias, B. M., Jarman, I., El-Deredy, W., & Lisboa, P. J. (2009, June). Grocery shopping recommendations based on basket-sensitive random walk. In Proceedings of the 15th ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 1215-1224).\
Available online: [Link](https://www.researchgate.net/profile/Paulo-Lisboa/publication/221653590_Grocery_shopping_recommendations_based_on_basket-sensitive_random_walk/links/09e4150cb9fb091a30000000/Grocery-shopping-recommendations-based-on-basket-sensitive-random-walk.pdf)
* Le, D. T., Lauw, H. W., & Fang, Y. (2017). Basket-sensitive personalized item recommendation. IJCAI.\
Available online: [Link](http://ink.library.smu.edu.sg/cgi/viewcontent.cgi?article=4767&context=sis_research)


