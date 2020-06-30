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
