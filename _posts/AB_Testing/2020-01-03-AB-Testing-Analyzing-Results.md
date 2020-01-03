# AB Testing - Lesson 5 - Analyzing Results

* Sanity Checks
* Single Metric
* Multiple Metrics
* Gotchas





## 1. Sanity Checks

**Reasons for Sanity Check** (example)

* Something goes wrong in the experiment diversion and the experiment and control aren't comparable
* You set up the filter differently in the experiment and the control 
* Your data capture didn't capture the events that you want to look for in your experiment



To do this, we should check 

* Population sizing metrics based on your unit of diversion  to make sure the population in experiment and control are comparable 
* The invariant metrics: metrics that shouldn't change when you run your experiment



### Choosing invariant metrics

**Example 1**

|                                                              | \# signed in users | \# cookies | \# events | \# CTR on "Start now" | \# Time to complete |
| :----------------------------------------------------------: | :----------------: | :--------: | :-------: | :-------------------: | :-----------------: |
| Changes order of courses in course list (unit of diversion: user-id) |         1          |     1      |     1     |           1           |                     |
| Changes infrastructure to reduce load time (unit of diversion: event) |         1          |     1      |     1     |           1           |                     |

* For case 1, 
  * \# signed in users is a good population sizing metric
  * \# cookies and \# events are also good population sizing metrics unless users in the experiment group tend to clear their cookies more often than the control group
  * "Start Now" button shows before the course list so \# CTR should remain the same
  * Putting easier course at first might reduce the time to complete for the users
* For case 2,
  * \# events is good population sizing metric since it's the unit of diversion
  * \# signed in users and \# cookies are randomly assigned due to the events so they are also the population sizing metrics
  * People press" Start Now" before viewing the videos 
  * \# time complete is difficult to track in event-based diversion ( users might be assigned to control and experiment multiple times)



**Example 2**

Change location of sign-in button to appear on every page

Unit of diversion: cookie

Which metrics should make good invariants?

* \# events , \# cookies, \# users are good population sizing metrics
* CTR on "Start Now": No, because sign-in button in home page will affect CTR on "start now"
* Probability of enrolling: No, because users enroll after signing in 
* Sign-in rate: No, this is what we're trying to change
* Video load time : Yes. This is back-end thing.



### Checking invariants

**Example 1**

Run experiment for 2 weeks

Unit of diversion: cookie

If you notice that \# total control is not equal to \# total experiment, how would you figure out whether this difference is within expectations?

Given: Each cookies is randomly assigned to the control or experiment group with probability 0.5



Answer:

Like flipping a fair coin: heads = control, the number of heads follows a binomial distribution 

* Compute standard deviation of binomial with probability 0.5 of success
  * SD = sqrt(0.5*0.5/(\# total control+\# total experiment))
* Multiply by z-score to get margin of error
* Compute confidence interval around 0.5
* Check whether the observed fraction of successes falls within the interval
* If not in the interval, one good thing to check is to look at the day-by-day data. Compute the fraction of control group in each day to figure out whether it's a overall problem or a specific day problem



What to do?

* Talk to the engineers
* Try slicing(e.g. by platform) to see if one particular slice is weird
* Check age of cookies - does one group have more new cookies





Common scenarios that experiment and control don't match up

* data capture
  * Capture a new experience the user is undergoing. Maybe the change is rare in experiment but more in control
* Experiment setup
  * Maybe you set up a filter for experiment only but not for the control 
* Infrastructure







## 2. Single Metric

**Example**

Change color and placement of "Start Now" button 

Metric: Click-through rate

Unit of diversion: cookie

Check the videos to see the details.



Usually we can run two test,

* Binomial normal approximation for empirical result
* Sign test - nonparametric test
* If these two test's results are different, we need to dig deeper(e.g. weekday effect of weekend effect)





**Example**

Simpson's paradox in CTR

* Looking at the total aggregate result, the experiment group has a lower CTR
* But if breaking it down by new users and experienced users, the result is on the contrary
* Possible reason: changes might affect the new user and experienced user differently





## 3. Multiple Metrics

**Example**

Prompt students to contact coach more frequently

Metrics:

* Probability that student signs up for coaching
* How early students sign up for coaching
* Average price paid per student

If Audacity tracks all three metrics and does three separate significance tests (alpha=0.05), what is probability at least one metric will show a significant difference if there is no true difference?
$$
P(FP=0)=0.95^3=0.857
$$

$$
P(FP\geq 1)=1-0.857=0.143
$$

Here we assume these three metrics are independent, but actually they are related. So this is an overestimate of the probability of a false positive.

* Probability of any false positive increases as you increase number of metrics
  * Solution: use higher confidence level for each metric
  * Method 1: Assume independence and set alpha separately for each metric
  * Method 2: Bonferroni correction
    * Simple
    * No assumptions
    * Conservative



**Example**

Update description on course list

Check the videos



Different strategies:

* Control probability that any metric shows a false positive
  * Family wise error rate (FWER)
* Control false discovery rate (FDR)
  * FDR=E[\# false positives / \# rejections]
  * Suppose you have 200 metrics, cap FDR at 0.05. This means you're okay with 5 false positives and 95 true positives in every experiment
  * If you want to detect a significant change across a number of metrics, cap the FDR instead of FWER







## 4. Conclusion

If you get significant result, ask yourself

* Do you understand the change	
  * When one is significant and the other one is not significant
  * When one is positive change while the other one is negative change
* Do you want to launch the change
  * Do I have statistically significant and practically significant results to launch the change
  * Do I understand what the change has done with the user experience
  * Is it worth it



### Gotchas

* Remove all the filters and do a ramp-up to see incidental change on the unaffected users that you didn't test in the original experiment









 