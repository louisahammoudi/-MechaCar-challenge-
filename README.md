

#Overview

AutosRUs' new MechaCar is "suffering from production troubles" and the company is hoping that an analytical review may help provide some insight. The goal of this project is to:

discover which variables predict the MPG for vehicle prototypes;
collect summary stats on the PSI of suspension coils;
determine if manufacturing lots are statistically different from the mean population;
design a study to compare the MechaCar performance against vehicles from other manufacturers.
```
library(dplyr)

MechaCarData <- read.csv(file='MechaCar_mpg.csv',check.names=F,stringsAsFactors=F)

lm(mpg ~ vehicle_length + vehicle_weight + spoiler_angle + ground_clearance + AWD,data=MechaCarData)

summary(lm(mpg ~ vehicle_length + vehicle_weight + spoiler_angle + ground_clearance + AWD,data=MechaCarData))
```

The script produced the following analysis:

![Linear Regression MPG](/Resources/linreg_mpg.PNG)

 The most significant variables in our dataset which show a non-random effect on the MPG of the MechaCar are the Vehicle Length and the Ground Clearance. As indicated by the yellow arrows in the image above, a linear regression model run on these variables against figures for MPG, resulted in p-values of 2.6x10-12 and 5.21x10-8, respectively. The intercept was also statistically significant, indicating that there are likely other factors, not included in our dataset, that have a strong impact on the MPG.
The slope of the linear model can not be considered to be zero, as the p-value of 5.35x10-11, indicated by the orange arrow above, is lower than even an extreme level of significance, and thus the null hypothesis must be rejected. This means that the relationship between our variables and the miles per gallon is subject to more than random chance.
Although there are still unconsidered factors, this model does predict the mpg of the MechaCar prototype with some relative effectiveness. The r-squared value of 0.7149, highlighted in the purple box, indicates that the model is 71% accurate... though it could probably do better.

## Summary Statistics on Suspension Coils
Using the dataset on suspension coils, I created a summary statistics table showing the suspension coil's PSI continuous variable across all manufacturing lots and the PSI metrics for each lot. To accomplish this, I first read the data csv file, created the summary table for all the manufacturing lots (the total_summary table), and then grouped the table by the manufacturing lot to produce the table for each lot (the lot_summary table).

While the overall variance, as shown in the Total Summary data above, is under 100 psi and meets specifications, there is a problem with one of the individual lots. As shown in the Lot Summary stats, the variance for Lot 3 is well over the acceptable threshold, at 170.28.

```
SuspensionData <- read.csv(file='Suspension_Coil.csv',check.names=F,stringsAsFactors=F)

total_summary <- SuspensionData %>% summarize(Mean=mean(PSI), Median=median(PSI), Variance=var(PSI), SD=sd(PSI), .groups = 'keep')

lot_summary <- SuspensionData %>% group_by(Manufacturing_Lot) %>% summarize(Mean=mean(PSI), Median=median(PSI), Variance=var(PSI), SD=sd(PSI), .groups = 'keep')
```

The script produced the following tables:

Total Summary Statistics

![Total Summary](/Resources/totalsummary.PNG)

Per Lot Summary Statistics

![Lot Summary](/Resources/lotsummary.PNG)

The design specifications for the MechaCar suspension coils dictated that the variance of the suspension coils must not exceed 100 pounds per square inch. Based on these specifications, the current manufacturing data does meet the criteria for all the lots in total as the variance is 62.29. Individually, Lots 1 and 2 meet the specifications with variances of 0.98 and 7.47 respectively, but Lot 3 has a variance of 170.29 which well exceeds the maximum of 100 pounds per square inch.

## T-Tests on Suspension Coils
I performed t-tests to determine if all manufacturing lots and each lot individually was statistically different from the population mean of 1,500 pounds per square inch.

```
#t-test for all manufacturing lots
t.test(SuspensionData$PSI, mu=1500)

#t-test for lot 1
t.test(subset(SuspensionData$PSI, SuspensionData$Manufacturing_Lot == "Lot1"), mu=1500)
#t-test for lot 2
t.test(subset(SuspensionData$PSI, SuspensionData$Manufacturing_Lot == "Lot2"), mu=1500)
#t-test for lot 3
t.test(subset(SuspensionData$PSI, SuspensionData$Manufacturing_Lot == "Lot3"), mu=1500)
```

The script produced the following analysis:

T-Test for All Manufacturing Lots

![T-Test All](/Resources/ttest_all.PNG)

The t-test for all manufacturing lots had a p-value of 0.06, meaning that there is enough evidence to reject the null hypothesis. Therefore, the mean PSI of all manufacturing lots is statistically different from the population mean of 1,500 pounds per square inch.

T-Test for Lot 1

![T-Test Lot 1](/Resources/ttest_lot1.PNG)

The t-test for Lot 1 had a p-value of 1, meaning that there is not enough evidence to reject the null hypothesis. Therefore, the mean PSI of Lot 1 is not statistically different from the population mean of 1,500 pounds per square inch.

T-Test for Lot 2

![T-Test Lot 2](/Resources/ttest_lot2.PNG)

The t-test for Lot 2 had a p-value of 0.61, meaning that there is not enough evidence to reject the null hypothesis. Therefore, the mean PSI of Lot 2 is not statistically different from the population mean of 1,500 pounds per square inch.

T-Test for Lot 3

![T-Test Lot 3](/Resources/ttest_lot3.PNG)

The t-test for Lot 3 had a p-value of 0.04, meaning that there is enough evidence to reject the null hypothesis. Therefore, the mean PSI of Lot 3 is statistically different from the population mean of 1,500 pounds per square inch.

## Study Design: MechaCar vs Competition
One of the most important factors that consumers consider when looking for a new car is the safety rating. It would be useful to perform a statistical study to compare the safety ratings of MechaCar vehicles against the safety ratings of vehicles from other manufacturers. The null hypothesis would be that MechaCar vehicles do not have statistically significant higher safety ratings than that of vehicles from other manufacturers, while the alternative hypothesis would be that MechaCar vehicles do have statistically significant higher safety ratings than that of vehicles from other manufacturers. The way to test this would be to use a one-tailed t-test which would determine if there is any difference between the groups being compared. We would not use a two-tailed t-test as we have a prediction about the direction of the difference. The data needed to run the statistical test is a table of MechaCar vehicles and their respective safety ratings, along with various other manufactureres and their cars with safety ratings.
