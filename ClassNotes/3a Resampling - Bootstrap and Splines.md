# 1. Introduction to Bootstrap
## 1.1 Required R packages 
- `broom` for extraction of model components
- `splines` for working with B-splines
- `tidyverse` 
- `tidymodels` for modeling framework
# 1.2 uncertainty in a Test Statistics
> testcase: let $p$ be the actual true proportion of customers who will use your company's coupon;
> to estimate $p$ you decide to take a sample of $n = 200$ and find that $x = 10$ or $\hat{p} =10/200=0.05=5\%$ redeemed the coupon
> 
## 1.2.1 Confidence Interval
$$
CI(p) = \hat{p} \pm 2 \cdot \text{SE}(\hat{p}) \\
= \hat{p} \pm 2 \sqrt{\frac{\hat{p}(1 - \hat{p})}{n}} \\
= 0.05 \pm 0.03
$$
- the margin of error (uncertainty) $\hat{p} \pm 2 \sqrt{\frac{\hat{p}(1 - \hat{p})}{n}}$ is inversely proportional to $\sqrt{n}$ -> the larger the sample size the less uncertainty there is the estimate
## 1.2.2 Bayesian Posterior Distribution
## 1.2.3 The Bootstrap
> it is a easy way to assess the uncertainty in a test statistic using *resampling*

- the idea is to simulate the data from the *empirical distribution* which puts a point mass of 1/n at each observed data point (i.e. sample the original data with replacement)
	- important to simulate n observations (same size as the original data) because the uncertainty in the test statistic is a function of n
# 2. Bootstrapping Regression Parameters
# 3. Non-linear Modeling via Basis Expansion
## 3.1 Piecewise Polynomials
A **piecewise polynomial** is a function that is made up of **different polynomial functions**, each applying to a specific interval or “piece” of the input space. Instead of using a single polynomial to model the entire dataset, piecewise polynomials divide the input into segments and fit a different polynomial to each segment.
> This can be useful when the relationship between the **input  x  and output  y  changes across different ranges of  x** , making a single polynomial insufficient for capturing the full pattern.

Types:
1. **Piecewise Constant**
	1. definition: in each segment the function takes a constant value (a horizontal line)
	2. **use case: if you only want to represent the average or majority behavior in each segment you can use piecewise constant functions**
	3. example: regresso-gram
		1. ![[Pasted image 20240916200540.png]]
2. **Piecewise Linear**
	1. definition: in each segment the function is a linear function (straight line with a slope)
	2. **use case: if you expect the data to follow straight-line relationships with each segment but with different slopes**
		1. ![[Pasted image 20240916200643.png]]
3. **Continuous piecewise linear**
	1. definition: This is similar to piecewise linear, but with the additional condition that the lines must connect smoothly at the boundaries between intervals. This means there are no jumps or sharp discontinuities at the points where the intervals meet.
	2. **use case: When you need a smoother fit across intervals and don’t want sharp jumps between segments.**
		1. ![[Pasted image 20240916200720.png]]****
4. **Piecewise linear basis functions**
	1. definition: In this approach, **basis functions** are used to construct a piecewise linear function. A basis function is a simple function (like a bump or spike) that is used to build a more complex function. For piecewise linear basis functions, we often use **“hinge functions”**.
		1. ![[Pasted image 20240916201106.png]]
## 3.2 B-Splines
### What is a B-spline?

A **B-spline** (Basis spline) is a type of spline, which is a **piecewise polynomial** function that is used to approximate or interpolate data. Unlike basic piecewise linear or polynomial functions, B-splines provide a smooth, flexible curve by joining multiple polynomial functions at points called **knots**. The main advantage of B-splines is that they offer **smoothness** at the boundaries between these pieces while keeping the flexibility of fitting complex shapes.
#### Key Characteristics of B-splines:
1. **Piecewise Polynomials**: B-splines consist of several polynomial functions defined over different intervals, with each polynomial valid only between specific **knots**.
2. **Local Control**: Each polynomial segment is controlled by a small number of nearby data points, which means changing one part of the curve doesn’t dramatically affect other parts (a property called local control).
3. **Smoothness**: B-splines are designed to ensure smooth transitions between intervals, meaning the function and its derivatives are continuous at the knot points.
4. **Degree**: The degree of the polynomial segments can be chosen (e.g., linear, quadratic, cubic), with cubic B-splines (third-degree polynomials) being very common due to their balance between smoothness and flexibility.
#### How B-splines Work
- **Knots**: The points at which the intervals for the polynomial pieces join are called **knots**. The placement of knots divides the data into segments where different polynomial functions are used.
- **Degree**: The degree of the polynomial determines the flexibility. For example:
  - Linear B-spline: Degree 1 (straight line segments).
  - Quadratic B-spline: Degree 2 (smooth curves).
  - Cubic B-spline: Degree 3 (even smoother curves).
  
- **Control Points**: These are the points that define the shape of the B-spline. Each control point influences the curve over a specific region of the spline, and changes in one control point have a limited effect on the overall curve.
- Visual Example
	- ![[Pasted image 20240916201800.png]]
