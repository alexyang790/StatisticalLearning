# 1. Supervised Learning Intro
## 1.1 Supervised Learning
- in supervised learning each observation can be partitioned into two sets: **the predictor(independent, feature) and outcome (response, dependent)**
	- Predictor -> $X$ 
	- Response -> $Y$ 
- Goal in SL is to **find patterns and relationships between $X$ and $Y$**
- *Unsupervised Learning* is different topics that have not any outcomes (i.e. no $Y$)
# 2. Example Data
![[Screenshot 2024-09-08 at 14.12.31.png]]
# 3. Linear Models
## 3.0 Summary
- *Linear Models* refer to a class of models where the output (predicted value) is a linear combination (weighted sum) of the input variables
	- $f(x;\beta)=\beta_0+\sum_{j=1}^{p} \beta_j x_j$
- the coefficients (or weights) $\hat{\beta}$ are often selected by minimizing the squared *training data*
## 3.1 Simple Linear Regression
### 3.1.1 Model Structure
- single predictor variable $x \in \mathbb{R}$
- prediction function: $f(x; \beta) = \beta_0 + \beta_1x$
- model parameters: $\beta = (\beta_0, \beta_1)$
### 3.1.2 Parameter Estimation
- List of terms 
	- **OLS** - ordinary least squares 
	- **SSE** - sum of squared errors
		- or **Residual sum of errors** 
	- **MSE** - mean sum of errors
	- **RMSE** - root mean sum errors
- SSE - sum of squared errors
	- $$ \begin{align} 
	SSE(\beta) &= \sum_{i}^{n}(y_i-f(x_i,\beta))^2 \\
	&= \sum_{i}^{n} (y_i-\beta_0-\beta_1x_i)^2 \\
	&= \sum_{i}^{n}\hat{\epsilon}_i^2
	 \end{align}$$
	 - where $\hat{\epsilon}_i=y_i-\hat{y}_i$ is the residual
	 - error here refers to the residual
- **The solutions are** 
	- $$\begin{align}
		\hat{\beta}_0&=\bar{y}-\beta\bar{x} \\
		\hat{\beta}_1 &= \frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i}^{n}(x_i-\bar{x})^2}
	\end{align}$$
- **Definitions:**
	- $$\begin{align}MSE(\beta)&=\frac{1}{n}SSE(\beta) \\ &= \frac{1}{n} \sum_{i=1}^{n}(y_i-f(x_i, \beta))^2\\RMSE&=\sqrt{MSE}=\sqrt{SSE}/\sqrt{n}\end{align}$$
## 3.2 OLS Linear Model in R 
### 3.2.1 Estimation with `lm()`
```R
model = lm(y~x, data = data_train)
summary(model) 
```
- **broom packages** provides three functions to make it easier to interact with model objects
	- `tidy()` summarizes information about model components
	- `glance()` reports information about the entire model 
	- `augment()` adds information about observations to a dataset
```r
library(broom)
broom::augment(ml)
```
### 3.2.2 Prediction with `predict()`
>the function 