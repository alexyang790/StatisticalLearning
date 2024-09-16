```R
# define simulation number 
sim_num = 100

# make a tibble to store results
library(tidyverse)
mse_f <- tibble(
  mse_linear = numeric(sim_num),
  mse_quad = numeric(sim_num),
  mse_cube = numeric(sim_num)
)

set.seed(613)

# repeating parts
for (x in 1:sim_num){
  # generate new data
  new_data <- sim_func(100, 3)
  
  # fit new models
  linear_model <- lm(Y ~X, data = new_data)
  quad_model  <- lm(Y~poly(X, degree = 2, raw = T), data = new_data)
  cube_model <- lm(Y~poly(X, degree = 3, raw = T), data = new_data)
  
  # predict using train_data
  train_data <- train_data %>%
    mutate(
      linear_pred = predict(linear_model, newdata = train_data),
      quad_pred = predict(quad_model, newdata = train_data),
      cube_pred = predict(cube_model, newdata = train_data)
    )
  
  # calculating MSE
  mse_f$mse_linear[x] <- mean((train_data$Y - train_data$linear_pred)^2)
  mse_f$mse_quad[x] <- mean((train_data$Y - train_data$quad_pred)^2)
  mse_f$mse_cube[x] <- mean((train_data$Y - train_data$cube_pred)^2)
}

# transform the long table to short
mse_f_long <- mse_f %>%
  pivot_longer(cols=everything(), names_to = "model", values_to = "mse")

# plotting
ggplot(data = mse_f_long, aes(x = mse, fill=model)) +
  geom_density(alpha = 0.4)
```