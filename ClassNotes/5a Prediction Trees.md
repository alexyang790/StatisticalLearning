Tree based methods: 
1. partition the feature space into a set of hyper rectangles 
2. fit a simple model (e.g. constant) in each region 

They are conceptually simple yet powerful: 
- main characters
	- flexibility 
	- natural graphical display
	- building blocks of random forest and boosting 
	- naturally includes feature interactions 
	- reduce need for feature transformations
- main implementations 
	- CART (classification and regression trees)
	- conditional inference trees (party package)

# 1.1 Prediction Trees 
![[Screenshot 2024-09-28 at 13.42.01.png]]
# 1.2 Building Prediction Trees 
> as usual we want to find the trees that makes predictions which minimizes some loss function 

- **classification trees** have class probabilities at the leaves (e.g. probability of heavy rain is 0.9)
	- e.g. loss = negative binomial likelihood
- **regression trees** have a mean response at the leaves (e.g. the expected amount of rain is 2 inches)
	- e.g. loss = mean square error
# 1.3 Recursive Binary Partition (CART)
>Because the number of possible trees is too large to exhaustively search, we usually restrict attention to recursive binary partition trees (CART).

