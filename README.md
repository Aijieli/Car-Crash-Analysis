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

The dataset is provided by the Department of Transportation in the City of Cary in North Carolina, and it includes information for all of the car crashes in 2014 – 2019 with 14 dimensions and 23137 observations. The dependent variable Crash_Score is a numeric variable, which measures the extent of the car crashes, based on a series of factors, such as the number of vehicles involved, and the number of injuries or fatalities. The distributions of Crash_Score and the log of Crash_Score, it is shown that the distribution of the Crash_Score is right-skewed, with a median of 5.660, and a max of 53.070, which indicate that most car crashes are slight car crashes and a small proportion are severe car crashes. The log of Crash_Score is left-skewed (Figure 1).

<img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/Crash%20Score%20Histogram.jpg" width="300" height="200"> <img src="https://github.com/Aijieli/Car-Crash-Analysis/blob/master/images/the%20Log%20of%20Crash%20Score%20Histogram.jpg" width="300" height="200">
Figure 1 Crash_Score Histogram and the log of Crash_Score Histogram
