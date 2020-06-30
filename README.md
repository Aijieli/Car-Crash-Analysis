# Car-Crash-Analysis

We aimed to analyze the Car Crash Dataset provided by the City of Cary in North Carolina, to identify factors that contribute to car crashes and to gain municipal management insights. The car crash dataset includes information such as the severity of the car crash, time, road condition, and traffic control condition. We analyzed the dataset by EDA and a series of ML methods. It is worth mentioning that the focus of our analysis is not to predict the severity of future car crashes, but to identify and interpret how and why different factors contribute to car crashes and provide insights for municipal management.<br>

Here are some key findings for our analysis:
- Interaction road feature, signal and stop traffic control, and curve road add to the Crash Score.
- Daytime has the highest Crash_Score, followed by late-early and overnight.
- The relative impact for interaction is lower for signal stop, and the relative impact for the work area is higher for late early.<br>

Here are the suggestions based on our findings:
- The intersection has complex traffic conditions as well as a significantly higher Crash_Score. Thus, municipal management should consider signal and stop, two-way protected median, and lighting as traffic control and road management practice.
- Time of day largely affects the Crash_Score. Thus, municipal management could try different practices for different times, especially in the work area during rush hour.
- Municipal management should provide better lighting during night and more road management for other roads.
- It is of equal importance to improving the road condition as well as to raising awareness for safety. Municipal management could consider adding more signs and slogans as reminders.

## Data Characteristics

The dataset is provided by the Department of Transportation in the City of Cary in North Carolina, and it includes information for all of the car crashes in 2014 – 2019 with 14 dimensions and 23137 observations. The dependent variable Crash_Score is a numeric variable, which measures the extent of the car crashes, based on a series of factors, such as the number of vehicles involved, and the number of injuries or fatalities. The distributions of Crash_Score and the log of Crash_Score, it is shown that the distribution of the Crash_Score is right-skewed, with a median of 5.660, and a max of 53.070, which indicate that most car crashes are slight car crashes and a small proportion are severe car crashes. The log of Crash_Score is left-skewed (Figure 1). Most of the independent variables are categorical variables, including year, month, time, road conditions, traffic control conditions, and weather.

<img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/Crash%20Score%20Histogram.jpg" width="300" height="200"> <img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/the%20Log%20of%20Crash%20Score%20Histogram.jpg" width="300" height="200"> <br>
**Figure 1 Crash_Score Histogram and the log of Crash_Score Histogram**

## Feature Transformation

We fitted a model with the unprocessed dataset, the in-sample R2 is relatively high, but the out-of-sample R2 is low. It seems that the model tends to overfit. Based on our observations, we decided to do a feature transformation and to combine some levels of the factors. Feature transformation could reduce the complexity of the model and prevent overfitting. Here is the process (Table 1):
Feature | Levels with Transformation
| ------------- | ------------- |
Time_of_Day | DAYTIME, OVERNIGHT, LATE-EARLY
Rd_Feature | INTERSECTION, OTHER
Rd_Character | STRAIGHT, CURVE
Rd_Surface | ASPHALT, OTHER
Weather | CLEAR-CLOUDY, RAIN-SNOW, OTHER
Traffic_Control | SIGNAL-STOP, OTHER

## Model Selection, Evaluation, and Interpretation

We used a series of techniques to select and evaluate generalized linear models (GLMs), and then interpret the model by the coefficients of variables. Although the predictive performance is not the main focus of our model, we used RMSE and out-of-sample R2 as the main performance metrics for us to select and evaluate models. The RMSE indicates how well the model can predict dependent variables in new datasets, and the out-of-sample R2 indicates how well the independent variables can interpret the variation for dependent variables in new datasets. We chose these metrics because we want the model to generalize insights, rather than overfit our dataset. Additionally, we applied 10-fold cross-validation, to utilize the dataset better and to provide performance metrics more accurately.

**Model 0: GLM with the unprocessed dataset**

*𝑔𝑙𝑚(𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒 ~ .,𝑔𝑎𝑢𝑠𝑠𝑖𝑎𝑛(),𝑑𝑎𝑡𝑎 = 𝑑𝑎𝑡𝑎)*

As we mentioned, we fitted a model with the unprocessed dataset, and the in-sample R2 is significantly lower than the out-of-sample R2, indicating overfit. As a result, we did feature transformation and reduced the level of factors.

**Model 1: GLM with the processed dataset**

*𝑔𝑙𝑚(𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒 ~ .,𝑔𝑎𝑢𝑠𝑠𝑖𝑎𝑛(),𝑑𝑎𝑡𝑎 = 𝑑𝑎𝑡𝑎2) 𝑔𝑙𝑚(𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒 ~ .,𝐺𝑎𝑚𝑚𝑎(𝑙𝑜𝑔="𝑙𝑖𝑛𝑘"),𝑑𝑎𝑡𝑎 = 𝑑𝑎𝑡𝑎2)*

We fitted a model with the dataset after feature transformation, and both the RMSE and out-of-sample R2 are improved. Then, we benchmarked the performance for different distribution functions and link functions. As we mentioned, the distribution of Crash_Score is skewed, and Gamma distribution could be a better choice. After we investigated the visualizations (Figure 2, 3), it seems that with Gamma distribution and log link, the residuals have zero means and constant variables, and there are normal distributions for most residuals but not for extremes. As a result, we chose the model with Gamma distribution and log link, but we will also use Gaussian distribution for interpretation and intuition.

<img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/GLM%20with%20Gaussian%20Distribution%201.jpg" width="400" height="200"> <img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/GLM%20with%20Gaussian%20Distribution%202.jpg" width="400" height="200"> <br>
**Figure 2: Visualization for GLM with Gaussian Distribution**

<img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/GLM%20with%20Gaussian%20Distribution%20and%20Log%20Link%201.jpg" width="400" height="200"> <img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/GLM%20with%20Gaussian%20Distribution%20and%20Log%20Link%202.jpg" width="400" height="200"> <br>
**Figure 3: Visualization for GLM with Gamma Distribution and Log Link**

**Model 2: GLM with feature selection** 
*𝑔𝑙𝑚(𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒 ~ 𝑅𝑑_𝐶𝑙𝑎𝑠𝑠 + 𝑇𝑟𝑎𝑓𝑓𝑖𝑐_𝐶𝑜𝑛𝑡𝑟𝑜𝑙 + 𝑅𝑑_𝐹𝑒𝑎𝑡𝑢𝑟𝑒 + 𝑇𝑖𝑚𝑒_𝑜𝑓_𝐷𝑎𝑦,𝑔𝑎𝑢𝑠𝑠𝑖𝑎𝑛(),𝑑𝑎𝑡𝑎 = 𝑑𝑎𝑡𝑎2)* <br>
*𝑔𝑙𝑚(𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒 ~ 𝑅𝑑_𝐶𝑙𝑎𝑠𝑠 + 𝑇𝑟𝑎𝑓𝑓𝑖𝑐_𝐶𝑜𝑛𝑡𝑟𝑜𝑙 + 𝑅𝑑_𝐹𝑒𝑎𝑡𝑢𝑟𝑒 + 𝑇𝑖𝑚𝑒_𝑜𝑓_𝐷𝑎𝑦,𝐺𝑎𝑚𝑚𝑎(𝑙𝑖𝑛𝑘="𝑙𝑜𝑔"),𝑑𝑎𝑡𝑎 = 𝑑𝑎𝑡𝑎2)*

After feature transformation and model selection, the in-sample R2 is still significantly higher than the out-of-sample R2. This is when we consider a feature selection to reduce the complexity of the model and prevent overfitting. We chose BIC forward selection and LASSO regression. The former starts with no variables in the model and adds the variable that gives the most statistically significant improvement of the fit. The latter starts with all the variables in the model and shrinks the parameters with the penalty. Additionally, let us emphasize the focus of the analysis, to identify and interpret the key factors that contribute to car crashes. With this goal, we chose BIC over AIC for and the penalty is greater for each additional parameter; we also chose LASSO over ridge, for it could shrink the parameters of the useless variables to zero.

Although BIC forward selection and LASSO regression not significantly improve the model performance, it helps us identify key factors, including Rd_Class, Traffic_Control. Rd_Feature, and Time_of_Day. Here are some findings:
- Interaction road feature adds to Crash_Score by approximately 0.36.
- Signal and stop traffic control add to Crash_Score by approximately 0.31.
- Daytime has the highest Crash_Score, followed by late-early, 0.30 lower, and overnight, 0.73 lower.

**Model 3: GLM with interaction** 
*𝑥𝑡𝑟𝑎𝑖𝑛=𝑚𝑜𝑑𝑒𝑙.𝑚𝑎𝑡𝑟𝑖𝑥(𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒 ~ .+ (.)^2,𝑡𝑟𝑎𝑖𝑛𝐷𝑎𝑡𝑎2)* <br>
*𝑦𝑡𝑟𝑎𝑖𝑛=𝑡𝑟𝑎𝑖𝑛𝐷𝑎𝑡𝑎2$𝐶𝑟𝑎𝑠ℎ_𝑆𝑐𝑜𝑟𝑒* <br>
*𝑔𝑙𝑚𝑛𝑒𝑡(𝑥_𝑡𝑟𝑎𝑖𝑛,𝑦_𝑡𝑟𝑎𝑖𝑛,"𝑔𝑎𝑢𝑠𝑠𝑖𝑎𝑛",𝑙𝑎𝑚𝑏𝑑𝑎 = 𝑙𝑎𝑠𝑠𝑜.𝑙𝑎𝑚.𝑚𝑖𝑛,𝑎𝑙𝑝ℎ𝑎 = 1)*

The GLM without interaction provides similar insights as EDA. We want to further improve the model and extract more insights from the dataset, so we decided to take interaction into consideration. Car crashes are complicated and different factors affect each other. In the following interaction plots, it is shown that there are interactions between factors (Figure 4).

<img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/Interaction%20for%20Different%20factors%201.jpg" width="400" height="200"> <img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/Interaction%20for%20Different%20factors%202.jpg" width="400" height="200"> <br>
**Figure 4: Interaction for different factors**

To achieve this, we included all the variables and interactions and applied high dimension LASSO regression. Additionally, we used 10-fold cross-validation, to determine the penalty parameter, lambda, with minimum MSE (Figure 5).

<img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/Selection%20for%20Lambda.jpg" width="400" height="200">
**Figure 5: Selection for Lambda**

With the high dimension LASSO regression, the in-sample R2 is significantly improved, and the out-of-sample R2 is further improved. It helps us investigate how a single factor contributes to car crashes as well as how multiple factors interact, which provides insights that could be overlooked or underestimated by traditional methods. Here are some findings:
- Although interaction, signal and stop both add to Crash_Score, the relative impact for interaction is lower for signal stop.
- The relative impact for the work area is higher for late early.
- The relative impact for light is higher during overnight.
- Road class and road condition interacts with each other. The ice, snow, slush road condition has relative high impact on other road than US and state highway.
