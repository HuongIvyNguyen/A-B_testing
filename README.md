# Design and Analyze A/B Testing: a case of Udacity Free Trial Screener
by Huong Ivy Nguyen, in fulfilling of Udacity's Data Analyst Nanodegree, Project 7

## Introduction (obtained from Udacity's instruction for this course)
In this project, we will be testing a new feature that Udacity added to their home page (at the time of the experiment). In particular, Udacity courses havw two options on the home page: "start free trial" and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead.

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.
The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## 1.Experimental Desgn 
### 1.1 Metric choice
+ **Invariant metrics**: Number of cookies, Number of clicks
+ **Evaluation metrics**: Gross conversion, Retention, and Net conversion

1. Number of cookies is the number of unique users to visit the course overview page. In this experiment, the Unit of Diversion is cookies, and it is evenly distributed among experimental and control group. Since the visits must happen before the users see the page indicated for the experiment, the number of cookies is independent from the experiment and thus can be chosen as one of the invariant metrics. 
2. Number of user-id is the number of users enrolled in the free trial. This is not an ideal metric for both evaluation and invariant metrics. This is because the number of users enrolled in the free trial can be fluctuated every day and thus we would not be able to obtain a good repeatability for the experimental results using this variable. 
3. Number of clicks is the number of cookies to click the start free trial botton. Since the action of clicking the free trial botton happens before the users see the experimental set-up, it should not be dependent on the experiment and thus is considered as one of the invariant metrics. One thing to be noted for this experiment is that the overview page is kept the same for both experimental and control groups until the users click the free trial botton.
4. Click-through probability is another good invariant metric since the clicks can be happened before the users see the experiment. Click-through probability is the number of unique cookies to click the 'start free trial' botton out of the number of unique cookies to the view the course overview page. 
5. Gross conversion is the number of users who complete the enrollment for the free trials over the number of users who clicked the "start free trial" button. This is a good evaluation metrics since it is directly related to the experimental set-up and it is dependent on the effect of the new feature added on the overview page. The underlying assumption here is that the gross conversion of the control group, which does not see the new feature added to the overview page, is higher than that of the experimental group, which will get to see a pop-up page asking them whether they will have enough time to devote for the course. 
4. Retention is the number of user-ids to remain enrolled for 14-day-trial period and make their first payment out of the total number of user-ids enrolled in the free trial. This is also another good evaluation metrics. As in the case of gross conversion, users in the experimental group are aware of the time commitment for the courses. Hence, those who choose to enroll in the free trials would more than likely to stay enrolled and finish the courses. That implies the retention might be higher for the experimental group than that for the control group. 
5. Net conversion is the number of user-ids remained enrolled for 14-day-trial period and at least make their first payment out of the number of users clicked the start free trial botton. This metric is dependent on the experimental setup and thus can also be used as one of the evaluation metrics. For the experimental group, since the users are aware of the time commitment required for the course, they can cancel their payment before the 14 days trial period. However, the control group is not aware of this time factor and could absolutely keep paying for the courses without noticing the minimum time requirement. This evaluation metric will help us identify whether having the "5 hours or more" botton would increase the ratio of users who make the payment over the total uders who see the start free trial botton. 

The overall expectation can be summerized as follow: 
+ The gross conversion of the control group is higher than that for the experimental group (please see explaination above)
+ The retention for the experimental group is higher than that for the control group since the former group is aware of the time commitment while the latter is not. 
+ The net conversion will not decrease significantly, which indicates whether or not the new added feature affect the revenues of the company.

### 1.3 Measuring Variability

**Data**:
+ Unique cookies to view page per day:	                40000
+ Unique cookies to click "Start free trial" per day:	3200
+ Enrollments per day:	                                660
+ Click-through-probability on "Start free trial":	    0.08
+ Probability of enrolling, given click:	            0.20625
+ Probability of payment, given enroll:	                0.53
+ Probability of payment, given click	                0.1093125

For the Bernoulli distribution with probability p and population N, the analytical stardard deviation is computed as stde = sqrt(p*(1-p)/N). In order to further understand whether the analytical estimates of standard deviation match the empirical standard deviation, we have to consider whether or not the unit of analysis and unit of diversion match up. 

#### Calculation for a given sample size:
**Sample size** is 5000 cookies visiting the course overview page.

##### Gross Conversion:
+ The baseline probability for gross conversion is 0.20625.
+ The number of users who see the start free trial page for this sample size is 5000 * 0.08 = 400 
Therefore, the stde of gross conversion is: sqrt(0.20625*(1-0.20625)/400) = 0.0202

The unit of analysis is the person who clicks the 'start free trial' button and the unit of diversion is a cookie that does so. They appear to be highly correlated though not the same since a person can use multiple devices to access the overview page to start the free trial. The high correlation signifies that the analytical estimate is accurate.  

##### Retention:
+ The baseline probability for retention is 0.53. 
+ The number of users who enrolled in the free trial for this sample size is 5000 * 0.08 * 0.20625 = 82.5
Therefore, the stde of retention is: sqrt(0.53*(1-0.53)/82.5) = 0.0549. 

The unit of analysis is a person who enrolled in the free trial and the unit of diversion is the user-id of that enrolled person. They are highly correlated as well, which indicates that the analytical estiamate is mostly accurate. It is very rare that these two variables won't match since it is less likely that a person would have several user-ids and vice versa. 

##### Net conversion:
+ The baseline probability for net conversion is 0.1093125
+ The number of users who see the start free trial page for this sample size is 5000 * 0.08 = 400 
Therefore, the stde of retention is: sqrt(0.1093125 * (1-0.1093125)/400) = 0.0156

The unit of analysis and the unit of diversion are both the same for the net conversion, which is a user-id that is enrolled and clicked the 'start free trial' button. The analytical estimate is expected to be mostly accurate. 

### 1.3 Sizing 
####**Number of Samples versus Power**
I chose to not use the Bernoulli correction for my analysis since the metrics for this test have high correlation and the Bernoulli correction would be too consertive for it. 

The number of samples needed for each metrics was calculated using the ![online calculator](http://www.evanmiller.org/ab-testing/sample-size.html) with alpha = 0.05, 1 - beta = 0.8. The number produced by the online calculator is per branch. Hence, in order to obtain the number of required page views for both expermental and control groups, the results obtained from the online calculator will need to be doubled. 

+ Gross conversion: The baseline conversion rate is 0.20625 or 20.625%, and d_min is 0.01 or 1%. The required number of samples calculated from the online calculator is 25835.  Hence, we would need 25835 / 0.08 * 2 = 645875 page views in order to 25835 clicks. 

+ Retention: The baseline retention rate is 0.53, and d_min is 0.01. The required number of samples calculated from the online calculator is 39115. Hence, we need 39115 / 0.08 / 0.20625 * 2 = 4741212 page views in order to get 39115 users who finished the 14-day-trial period.

+ Net conversion: The baseline conversion rate is 0.1093125, and d_min is 0.0075. The required number of samples calculated from the online calculator is 27413. Hence, we would need 27413 / 0.08 * 2 = 685325 page views to get 27413 clicks on the 'start free trial' button.

Therefore, we would need 4741212 pageviews (the largest value of all three evaluation metrics) for this sample size. HOwever, this is a very large numver of required pageviews since in order to obtain 4741212 pageviews, it takes 4741212/40000 = 118 days, which is too long. Therefore, a better choice would be to drop the retention rate for the final evaluation metrics list. The final number of pageviews needed would be 685325. 

####**Duration versus Exposure**

With daily traffic of 40000 pageviews per day, I would direct 70% of my traffic (28000) to the experiment, which means it should take us approximately 685325/28000 ~ 25 days to accomplish the experiment. 

The 70% of traffic being redirected means that 35% would go to the control group and 35% to the experimental group. This fration would not affect the whole operation of exisiting customers as well as the content of Udacity's webpage. 

## 2.Experimental Analysis (![data](https://docs.google.com/spreadsheets/d/1Mu5u9GrybDdska-ljPXyBjTpdZIUev_6i7t4LRDfXM8/edit#gid=0))

### Sanity Checks
Both the experimental and control groups are assigned to have a 0.5 Bernoulli distribution probability. Hence, their standard deviation is sqrt(0.5*0.5/N1 + N2) where N1, N2 are the population of the experimental and control groups. The margin of error for a 95% confidence interval is 1.96 * std. 

#### Number of cookies:
+ Total pageviews for the control groups: 345543 
+ Total pageviews for the experimental group: 344660
+ Total pageviews: 690203
+ Probability of cookie in both groups: 0.5
+ stde = sqrt(0.5*(1-0.5)/(345543+344660)) = 0.0006018
+ margin of error = 1.96*stde = 1.96 * 0.0006018 = 0.001180
+ 95% confidence interval = [0.5-0.001180, 0.5+0.001180] = [0.49882, 0.50118]
+ observed = 345543/690203 = 0.5006

Since the observed value is within the calculated confidence interval, this invariant metric passed the sanity check.

#### Number of clicks:
+ Total clicks for the control groups: 28378
+ Total clicks for the experimental group: 28325
+ Total clicks: 56730
+ Probability of cookie in both groups: 0.5
+ stde = sqrt(0.5*(1-0.5)/56730) = 0.0021
+ margin of error = 1.96*stde = 1.96 * 0.0021 = 0.0041
+ 95% confidence interval = [0.5-0.0041, 0.5+0.0041] = [0.4959, 0.5041]
+ observed = 28378/56730 = 0.5005

Since the observed value is within the bounds, this invariant metric passed the sanity check.

### Effect Size Tests

The probability difference is computed as follow:

d = X_exp / N_exp - X_cnt / N_cnt
where: 
* X_exp is the number of target samples of the experimental group
* N_exp is the number of total samples of the experimental group
* X_cnt is the number of target samples of the control group
* N_cnt is the number of total samples of the control group

The pooled probability and pooled standard error are calculated as follow:

p_pooled = (X_cnt + X_exp) / (N_cnt + N_exp)
se_pooled = sqrt(p_pooled * (1-p_pooled) * (1/N_cnt + 1/N_exp))

The lower and upper bounds are calculated as:
lower = d - se_pooled
upper = d + se_pooled

**Gross Conversion**

N_cnt = clicks_control = 17293.
X_cnt = enrollment_control = 3785.
N_exp = clicks_experiment = 17260.
X_exp = enrollment_experiment = 3423.

p_pooled = (X_cnt + X_exp) / (N_cnt + N_exp) = 0.2086
se_pooled = sqrt(p_pooled * (1-p_pooled) * (1./N_cnt + 1./N_exp)) = 0.00437

d = X_exp / N_exp - X_cnt / N_cnt = -0.02055

lower = d - se_pooled = -0.0291
upper = d - se_pooled = -0.0120
Since the interval does not contain 0, the metric is statistical significant. And since tt does not include d_min = 0.01 or -d_min = -0.01 either, it is also practical significant.

**Net conversion**

N_cnt = clicks_control= 17293.
X_cnt = payment_control = 2033.
N_exp = enrollment_experiment = 17260.
X_exp = payment_experiment = 1945.

p_pooled = (X_cnt + X_exp) / (N_cnt + N_exp) = 0.1151
se_pooled = sqrt(p_pooled * (1-p_pooled) * (1./N_cnt + 1./N_exp)) = 0.00343

d = X_exp / N_exp - X_cnt / N_cnt = -0.0048

lower = d - se_pooled = -0.0116
upper = d + se_pooled = 0.0019
Since the interval contains 0, it is not statistical significant, and therefore it is not practical significant.

### Sign Tests:

All calculations were performed using the ![online calculator](http://graphpad.com/quickcalcs/binomial1.cfm) 

**Gross conversion**
+ Number of success: 4, which is the number of days that we see an improvement in the number of enrollments in the experimental group.
+ Number of trials: 23, which is the toal 23 days of the experiments. 
+ Probability: 0.5
+ Two-tailed p-value: 0.0026

Since the two-tailed p-value is smaller than alpha = 0.05, the change is statistically significant. 

**Net conversion**
+ Number of success: 10, which is the number of days that we see an improvement in the number of payments in the experimental group.
+ Number of trials: 23, which is the toal 23 days of the experiments. 
+ Probability: 0.5
+ Two-tailed p-value: 0.6776

Since the two-tailed p-value is larger than alpha = 0.05, the change is not statistically significant. 

## Summary:

Both the effective size test and the sign test indicate that the new added feature will practically significantly reduce the gross conversion though there is no effect the net conversion gained from it. 

## Recommendation:

My recommendation is to not implement this new feature into the overview page of Udacity. This is because the result shows that the net rate conversion would not be increased practically significant by adding this new feature. In other words, adding the time commitment alert feature would not help to increase the number of paid users, which goes against the original goal of Udacity.

## Follow-up Experiment:
Another feature such as 'first-time user discount' can be added to attract attention from students who are already determined to take the course but are hestiated to pay. One thing to note here is that this new button 'first-time user discount' button is added in addition to the free trial button. 
There are two different hypothesises that can be made here:
+ By providing a discount program for first-time users, it will help Udacity to attract more students to enroll into the courses.
+ Even though these enrolled user-ids will be enrolled at a lower price than the others, the number of user-ids who are enrolled will increase over time, which will help to increase the revenue of Udacity overall. 

There are two evaluation metrics that can be used for each of these hypothesis:
+ Conversion rate, which is the number of homepage viewers who became enrolled users after clicking the discount program button out over the total number of enrolled users. 
+ Revenue gain, which is the ratio of revenue over the number of homepage viewers who became enroller users after clicking the discount program. 

This first evaluation metric will allow us to evaluate whether or not the additional option hlpes boosting the number of enrollments. The second evaluation metric will test whether adding this new option helps to increase the revenue of Udacity. 
The invariant metric is the number of cookies since the unit of diversion is still the a cookie. This choice is due to the fact that the homepage viewers might not sign in. However, they are signed in then the user-id will be used instead. 
