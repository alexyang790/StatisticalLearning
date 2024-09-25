# 1. Predictive Performance
## 1.1 Model Complexity
> most model families can be tuned to have complexity or EDF (effective degrees of freedom)

examples: 
- number of parameters in a linear model 
- neighborhood size in knn models 
- penalty $\lambda$ or constraint t for regularized models 
- number of trees, tree depth (classification and regression trees, random forest, boosting)

The goal is to tuned the model structure so that it is just flexible enough to capture the true structure but not so much that it overfits
- optimal complexity for a given training data set depends on the sample size

## 1.2 Tuning parameters
> tuning parameters are like the control knobs of a model 

examples:
- some tuning p directly controls the complexity or flexibility of a model (k in knn)
- some impact the structure or algorithm of a model (Eucildean or Manhattan distance in knn)
- pre-processing, feature engineering, and imputation approaches
	- some implementations can scale data internally before estimating parameters (e.g. glmnet())
	- dropping values with NA or use median imputation 

Given the values of the tuning parameters, it is often straightforward to estimate the model parameters from the data. **We use $\omega$** 

## 1.3 Predictive Performance
 $$ \omega_{\text{opt}} = \arg \min_{\omega} \text{EPED}(\omega|\text{train}) $$
 - to find $\omega_{\text{opt}}$ optimal tuning parameter
 - we need to minimize **EPE (expected prediction error, or the average prediction error of a model)** using tuning parameter given data
# 2. Estimating Predictive Performance
## 2.1 Model Selection 
> the goal of model selection is to pick the model that will provide the best *predictive performance* i.e. model with smallest EPE
> - note: the "model" here refers to the model family plus tuning parameter 

**Two main approaches to estimating the EPE of a model**
1. predict on hold-out data 
2. make a mathematical adjustment to the training error that better estimates the test error 

## 2.2 Hold out set (Train Test Split)
> an obvious way to assess how well a model will perform on a test data is to evaluate it on hold-out data 
![[Pasted image 20240918184008.png]]

**Steps:** 
1. pre-precess the training data so it's suitable for the chosen model. This produces transformed training data 
	1. record any parameters were learned estimated 
2. fit/train models using the training data. This estimates the model parameteres and produces a prediction function 
3. apply the same pre-processing steps to the test data from the step 1 
4. make predictions on the test data 
5. evaluate predictive performance on the test data

![[Pasted image 20240918185534.png]]

**Problems with using a single hold-out set:** 
1. 