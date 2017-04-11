# Design and Analyze A/B Testing: a case of Udacity Free Trial Screener
by Huong Ivy Nguyen, in fulfilling of Udacity's Data Analyst Nanodegree, Project 7

## Introduction (obtained from Udacity's instruction for this course)
In this project, we will be testing a new feature that Udacity added to their home page (at the time of the experiment). In particular, Udacity courses havw two options on the home page: "start free trial" and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead.

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.
The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## Experimental Desgn 
### 1.1 Metric choice
+ Invariant metrics: Number of cookies, Number of clicks
+ Evaluation metrics: Gross conversion, Retention, and Net conversion

* Number of cookies is the number of unique users to visit the course overview page. In this experiment, the Unit of Diversion is cookies, and it is evenly distributed among experimental and control group. Since the visits must happen before the users see the page indicated for the experiment, the number of cookies is independent from the experiment and thus can be chosen as one of the invariant metrics. 
* Number of clicks is the number of cookies to click the start free trial botton. Since the action of clicking the free trial botton happens before the users see the experimental set-up, it should not be dependent on the experiment and thus is considered as one of the invariant metrics. One thing to be noted for this experiment is that the overview page is kept the same for both experimental and control groups until the users click the free trial botton. 
* Gross conversion is the number of users who enrolled in the free trials over the number of users who clicked the start free trial botton. This is a good evaluation metrics since it is directly related to the experimental set-up and it is dependent on the effect of the new feature added on the overview page. The underlying assumption here is that the gross conversion of the control group, which does not see the new feature added to the overview page, is higher than that of the experimental group, which will get to see a pop-up page asking them whether they will have enough time to devote for the course. 
* Retention is the number of user-ids to remain enrolled for 14-day-trial period and make their first payment out of the total number of user-ids enrolled in the free trial. 

