# 01 - Bootstrapping & Hypothesis Testing

## Date: July 16th, 2024

```{r}
data = read.csv("/Users/alexyang/Git/Alex-Yang-MSDS/STAT 6021/ClassData.csv")
```

```{r}
library(tidyverse)
library(ggplot2)
```

```{r}
ourdata5 <- data %>%
  mutate(slp=as.numeric(Sleep_Hrs))
```

```{r}
t.test(ourdata5$slp) #one sample t-test
```

## Bootstrapping (random sample replacement)

### Mean Function (removing nas)

```{r}
mean.calc <- function(x){
  return(mean(x, na.rm = TRUE)) #adding portion to remove the NAs
}
```

### Sample Replacements for the 68 students

what does replace = TRUE do? It will have the same sample size but some records might be duplicated to act as if there are more than one sample population

```{r}
bootstrap.means <- replicate(10000, #repeating the two lines below 10000 times
                             {
bootstrap.data <- sample(ourdata5$slp, replace = TRUE) #picking out randonmly 
mean.calc(bootstrap.data) #calculate the means for bootstrap data
}
)
```

```{r}
bootstrap.df <- data.frame(bootstrap.means) #framing bootstrap.means into a df
```

```{r}
ggplot(bootstrap.df,
       aes(x=bootstrap.means)) + geom_density() #plotting bootstrap.means

```

### Finding 95% confidence interval for the boostrap.means

```{r}
quantile(bootstrap.means, c(0.025, 0.975)) 
```

# Hypothesis Testing

-   a hypothesis is a supposition or proposed explanation made on basis of limited evidence as a starting point for further investigation

    -   Purpose of hypothesis testing is to determine if that is true

-   elements of hypothesis testing

    -   null hypothesis H0 aka "status quo"

    -   alternative hypothesis HA, represents the values of a population parameter for which the researcher wants to gather evidence to support. Contains the value of parameter we consider plausible if we reject null

    -   p-value is probability of obtaining a test statistic more extreme than actual sample value

    -   write conclusion of the hypothesis test

-   two types of error

    -   type I error: H0 is True but reject H0

    -   type II error: H0 is NOT True but do NOT reject H0 (accept H0) (worse than type I)

        -   level of significance, alpha, is the likelihood of making a Type I error

## Testing Hypothesis about Probability

```{r}
prop.test(40, 100, p=0.85, alternative ="l")
```

this is the portion we look at:

```         
data:  40 out of 100, null probability 0.85
X-squared = 155.31, df = 1, p-value < 2.2e-16
```

-   since our p value is less than 5% or this case 1% always reject the null

    -   statistically significant result

## Insert slides from 28

## Testing Hypothesis about Means

```{r}
t.test(ourdata5$slp, mu=6, alternative = 'greater') #mu is target mean H0 mu
```

# 02 - AB Testing

## Comparing Two Sample Probability

```{r}
prop.test(c(7,15), c(15,19))
```

what we are looking for:

```         
95 percent confidence interval:
 -0.69445522  0.04884118
```

insert photo

```{r}
nba = read.csv("nba.csv")
```

```{r}
table(nba$W.L, nba$home_away)
```

```{r}
prop.test(c(518,712), c(1230, 1230))
```

> We are 95% confident that the winning proportion for all nba games played at home is between 0.118 and 0.198 higher than when the game is played away

```{r}
library(ggplot2)
ggplot(nba, aes(x=nba$home_away, fill = nba$W.L)) +geom_bar(position ='fill')
```

## Comparing Two Sample Means

> assumption: the two populations have to be independent of each other. If assumption is violated, the assumption of independence fails.

question: comparing average points won between home and away games

```{r}
t.test(PTS~home_away , data = nba) #the ~ refers to a categorical var
#we can see below that it separates between the two populations (home vs away)
```

Note: **t.test when you have numerical variable, prop.test when you have categorical variable (win or loss)**

> we are 95% confident that the average number of points scored in all nba home games is between 1.15 and 3.06 points higher than all nba away games

```{r}
nba %>% ggplot(aes(x=home_away, y=PTS), fill = home_away)+geom_boxplot()
```

## Comparing Two Paired Sample Data

> the two sample pool data are matched with each other (e.g. father's age to match with a specific mother's age). We cannot do t.test (we can but it will be less powerful). We look at the difference in the paired data

Use the difference between two paired data and treat it as one sample. Then do a confidence interval for that one sample

```{r}
Ourdata <- read.csv("/Users/alexyang/Git/Alex-Yang-MSDS/STAT 6021/01/ClassData.csv")
```

```{r}
#cleaning data 
library(dplyr)
Ourdata2 <- mutate(Ourdata, Age_Diff=
                     as.numeric(
                       gsub(" years", "", Ourdata$Age_Diff_Parents)
                       )
                   )
```

```{r}
t.test(Ourdata2$Age_Diff)
```

## Comparing More than Two Means (ANOVA)

-   two steps:

    1.  is there a difference between all the three means
    2.  if there is which means are different

ANOVA is a hypothesis testing:

-   H0: all the group means are the same

-   HA: at least one of the population means is different

-   need to find the p-value turns out to be less than alpha (accepted error level) then reject H0

-   test statistics -\> F test

    -   what it does is that it measures the variance between the groups and the variation within each of the groups

    -   one way to measure the variation between three groups is the mean square between/mean square within F=MSB/MSW

```{r}
CT <- read.csv("/Users/alexyang/Git/Alex-Yang-MSDS/STAT 6021/Clinical_trial.csv")
```

```{r}
library(ggplot2)
ggplot(CT, aes(x=Drug, y=Pain_Rating, fill=Drug)) + geom_boxplot()+geom_jitter()
```

```{r}
anova <- aov(Pain_Rating~Drug, data = CT)
```

```{r}
summary(anova)
```

-   Total variation (sum of square total SST) = variation between groups + variation within groups

    -   SST = SSB + SSW

-   F = MSB/MSW if F =1 then can't reject H0 (groups are different); if F \> 5 then we can reject H0

```{r}
# method to compare variance between family of samples 
TukeyHSD(anova, conf.level = 0.95)
```

```{r}
plot(TukeyHSD(anova, conf.level = 0.95))
```

Conclusion: We are 95% confident that the mean pain rating for drug A is much lower compare to Drug B from 0.82-... [repeat] Because of that we have statistical evidence to conclude that the mean pain rating for drug a is lower than drug b and c. From this experiment we can make the conclusion that the ...

# 03 - Introduction to Predictive Modeling

```{r}
startup <- read.csv('Startups.csv')
```

if you want to preview and see if there's statistical relationships - use scatterplot.

```{r}
library(ggplot2)
ggplot(
  startup, 
  aes(x=R.D.Spend, y=Profit)
) + geom_point()
```

## Simple Linear Regression (SLR)

(Refer to the notebook for notes)

## Multiple Linear Regression (MLR)

(Refer to the notebook for notes)

## Using R for Predictive SLR

```{r}
library(ggplot2)
ggplot(startup, aes(x=R.D.Spend, y=Profit)) +
  geom_point() +
  geom_smooth(method = 'lm')
```

```{r}
cor(startup$Profit, startup$R.D.Spend) #finding the correlation (strength of association, refer to notes for more detail)
```

```{r}
sd(startup$Profit)

```

```{r}
sd(startup$R.D.Spend)
```

```{r}
b1 <- (cor(startup$Profit, startup$R.D.Spend)*sd(startup$Profit))/(sd(startup$R.D.Spend))
```

A function that does all the above

```{r}
mod <- lm(Profit~R.D.Spend, data = startup)
print(mod)
```

## Bootstrapping for Parameter Estimation

```{r}
startups <- read.csv('Startups.csv')
```

```{r}
library(tidyverse)
```

```{r}
ggplot(startups, aes(x=R.D.Spend, y=Profit)) + geom_point() +geom_smooth(method = lm, se=FALSE, color = 'red')+
  geom_abline(data =bootstrap_estimates, aes(intercept=X.Intercept., slope = R.D.Spend), color='pink')
```

```{r}
model1 <- lm(Profit~R.D.Spend, data=startups)
coef(mode1)
```

**BOOTSTRAPPING**

-   take random samples with replacement

-   for each sample you take use the lm() method

-   and then repeat a lot of time and each time it returns the slope and y intercept

-   you then take the average slope and the average y intercept (that'll give you a more robust estimate)

```{r}
bootstrap_estimates <- replicate(1000, {
bootstrap_samples <- startups[sample(1:nrow(startups), nrow(startups), replace=TRUE),]
bootstrap_models <- lm(Profit~R.D.Spend, data=bootstrap_samples)
coef(bootstrap_models)
})

bootstrap_estimates <- data.frame(t(bootstrap_estimates))
bootstrap_estimates
```

```{r}
summarize(bootstrap_estimates, mean_b0=mean(X.Intercept.), mean_b1=mean(R.D.Spend))
```

**Multiple Regression Models**

```{r}
model2 <- lm(Profit~R.D.Spend+Administration+Marketing.Spend, data=startups) #this is something different
coef(model2)
```

## The old school way (what the lm function actually does)

**Finding the design matrix for startups**

```{r}
#column binding, have to make sure every column is the same length. Outputs a matrix 
X<-cbind(1, startups$R.D.Spend, startups$Administration, startups$Marketing.Spend)
```

```{r}
Y <- as.matrix(startups$Profit)
```

```{r}
XtX <- t(X)%*%X
```

```{r}
inverse_XtX <- solve(XtX)
```

```{r}
XtY <- t(X)%*%Y
```

```{r}
beta <- inverse_XtX%*%XtY
```

```{r}
print(beta)
```

# 04 - Model Assumptions and Residual Analysis

> how to check if the linear model is a good model and could create a good linear relationships (if assumptions are met)

-   error terms (have a normal distribution) -\> N(0, sigma\^2)

The assumptions of linear models are:

1.  linearity assumption (scatterplot and residuals spread)
2.  independence assumption (stock prices are related by the day -\> example where independence fails)
3.  equal variance assumption (in sample -\> randomly scattered residual plot)
4.  normal population assumption (all the error terms are normally distributed, QQ plot)

## Making Scatterplot use Faucet Wrap

```{r}
#load tidyr part of tidyverse
library(tidyverse)

startup <- read.csv('Startups.csv')

#transform dataset to make sure var names are in cells 
long <- gather(startup, key="predictor", value="value", 
               R.D.Spend, Administration, Marketing.Spend) #create a variable entry for R.D. 50 times, variable entry to administration 50 times and ... 

#make plots using long table
ggplot(data = long, aes(x=value, y=Profit, color=predictor)) +geom_point()+
  facet_wrap(~predictor,scale = "free_x") #free_x frees the range between different graphs
```

```{r}
model1 <- lm(Profit~R.D.Spend+Administration+Marketing.Spend, data = startup) #when listing predictors use +
coef(model1)
```

```{r}
startup_prediction <- mutate(startup, predictions=fitted(model1), resid=residuals(model1)) #fitted uses the prediction from the model to create predictions 
```

## Residual Random Distribution

```{r}
ggplot(startup_prediction, aes(x=predictions, y=resid)) + geom_point() + geom_hline(yintercept = 0, color ='red')
```

## Normality Assumption (QQ Plot)

QQplot will tell if residuals follow normal distribution

```{r}
ggplot(startup_prediction, aes(sample=resid))+stat_qq()+stat_qq_line(color = 'red')

#QQplot assumes the sample data follows some kind of distribution. On the y-axis is the theoritcal distributino of the variable. On x-asix is in theory what they should look like if they follow the normal distribution. If normal distribution the line should follow a diag line
```

## What happens when one or more assumption fails

```{r}
planets <- read.csv('PlanetsData.csv')
View(planets)
```

```{r}
ggplot(data = planets, aes(x= distance, y= revolution)) +geom_point()
```

**add slides from class**

## Fit Models into Prediction

```{r}
library(tidyverse)
startups <- read.csv('Startups.csv')
```

```{r}
model1 <- lm(Profit~R.D.Spend, data = startups)
coef(model1)
```

```{r}
new_dat <- data.frame(R.D.Spend= c(165349.20, 500000))
prediction <- predict(model1, newdata = new_dat)
prediction
```

**Prediction interval and confidence interval calculation**

```{r}
predict(model1, newdata = new_dat, interval = 'predict', level = 0.95) #prediction for profit when R.D. is one individual company
```

```{r}
predict(model1, newdata = new_dat, interval = 'confidence', level = 0.95) #predict average profit
```

**Prediction interval and confidence interval calculation for transformed models**

```{r}
library(tidyverse)
planets<-read.csv("PlanetsData.csv")
planets2<-mutate(planets, log_dist=log(distance), log_rev=log(revolution))
model3<-lm(log_rev~log_dist, data=planets2)
coef(model3)
```

```{r}
new <- data.frame(log_dist = c(log(93), log(193)))
predict <- predict(model3, new, interval = 'prediction')
```

```{r}
predict
```

```{r}
exp(5.901693) #untransform it 
```

## Variation in Models

**insert slide number 8/10**

## More on the independence assumption...

> if two predictors have strong correlation we shouldn't put both in the same linear model. Then we should get rid of one of them, how? We'll discuss more below

1.  correlation matrix - pairwise comparisons between predictors should be below 0.80
    -   covariance matrix
    -   correlation

```{r}
library(tidyverse)
```

```{r}
startups <- read.csv('Startups.csv')
```

```{r}
startups2 <- startups[,-4] #drop the 4th column
```

```{r}
#creating correlation matrix
cor_mat <- round(cor(startups2), 2) 
cor_mat #for example R.D.Spend and Marketing.Spend have rather strong correlation 
```

```{r}
#to better visualize the previous table regarding to correlation
library(ggcorrplot)

ggcorrplot(cor_mat, lab=T, type='lower')
```

2.  tolerance: the influence of one predictor variable on all other predictor variables. It's defined as T = 1-R\^2. If T\<0.1 then there might be multicolinearity issues and with T\<0.01 there certainly is multicolinearity
3.  Variance Inflation Factor (VIF): VIF = 1/T. With VIF \>5 there might be multicolinearity present. If VIF \>10 then multicolinearity is definitely present

Finding VIF in R

```{r}
model10 <- lm(data = startups, Profit~R.D.Spend+Administration+Marketing.Spend)
```

```{r}
summary(model10)
```

```{r}
library(car)
```

```{r}
vif(model10)
```

# 01 - Inference on the linear model; interpreting the model

1.  Testing multiple linear regression
2.  Interpreting the output
3.  model complexity and underfitting/overfitting

```{r}
startups <- read.csv('Startups.csv')
```

```{r}
model1 <- lm(Profit~R.D.Spend+Administration+Marketing.Spend, data = startups)
summary(model1)
```

> we see from the analysis on the F-statistic (ANOVA for usefulness) is almost 0 so we reject H0 that the model is useless. Then we can transition to individual t-test

```{r}
model2 <- lm(Profit~R.D.Spend+Marketing.Spend, data = startups)
summary(model2)
```

## Stepwise Regression

how to automate this process? - Stepwise Regression (two stages): forward selection, backward elminiation

```{r}
#AIC keep adding variable to see if more noise if more then drop if not then keep
library(MASS)
model3 <- lm(Profit~R.D.Spend+Administration+Marketing.Spend, data = startups)
aic <- stepAIC(model3, direction = 'both')
```

```{r}
summary(aic)
```

```{r}
summary(model1)
library(car)
```

`{r}5avPlots(model1)}`

```{r}
set.seed(123)
```

```{r}
sd(rnorm(100, 15, 5))
```

```{r}
sd(log(rnorm(100,15,5)))
```

```{r}
set.seed(123)

random_numbers <- runif(100, min = 0, max = 1)
sd(random_numbers)
```

```{r}
sd(log(random_numbers))
```

```{r}
# Set seed for reproducibility
set.seed(123)

# Generate 100 random numbers from a normal distribution with mean 0.5 and standard deviation 0.1
# Note: Adjust the standard deviation as needed to ensure most values fall between 0 and 1
mean <- 0.5
std_dev <- 0.1
random_numbers_normal <- rnorm(100, mean = mean, sd = std_dev)

# Truncate the values to be between 0 and 1
random_numbers_truncated <- pmin(pmax(random_numbers_normal, 0), 1)

# Generate a scatter plot
plot(random_numbers_truncated, main = "Scatter Plot of 100 Truncated Random Numbers (Normal Distribution)",
     xlab = "Index", ylab = "Random Number", pch = 19, col = "blue")

```

