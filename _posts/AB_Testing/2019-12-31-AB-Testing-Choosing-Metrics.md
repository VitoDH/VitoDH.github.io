# AB Testing - Lesson 3 - Choosing Metrics



## 1. Definition

### Invariant Checking

The metrics that shouldn't change across your experiment and your control

* Is total population the same
* Is the distribution the same
* You can use multiple metrics at this stage



### Evaluation

*  High level business metrics
  * How much revenue you make
  * What your market share is 
  * How many users you have
* Detailed Metrics
  * User experience with actually using your product
    * How long they stay on your page
* Number of metrics to use depend on the culture of the company



### How to define metrics?

* Come up with a high level concept for a metric
  * One sentence summary that everyone can understand like "Click Through Probability", "Active User", etc
* Figure out all of the nitty gritty details
  * What does active mean?
* Take all the data measurements and summarize them into a single metric
* You can group all metrics into a composite metrics using **Overall Evaluation Criterion**





## 2. Expanding the Customer Funnel

Here we use the example of the hypothetical company "Audacity".

* Homepage visits
* Exploring the site
  * \# users who view course list
  * \# users who view course details
* Create an account
  * \# users who enroll in a course
  * \# users who finish lesson 1, lesson 2, etc
  * \# users who sign up for coaching at various levels
* Completing a course
  * \# users who enroll in a second class
  * \# users who get jobs



Things to note:

* Different platforms: IOS, Android, Desktop
* Each stage is a metric: Count - \# users who reach that point
* Rates/Probability: \# (unique) users in current level / \# (unique) users in previous level
  * want to increase rate of progression
* **Rates are often better than probabilities for measuring the usability of a button**



Metrics that are **difficult** to measure:

* Don't have access to data
* Takes too long to calculate the metrics: e.g. whether student get a job



## 3. Other techniques to define metrics

* External data
  * Survey
  * Academic Research



* Internal data
  * Use all of the existing data, e.g. logs
    * Retrospective/Observational analyses, running experiments
  * Gather new data: you can't answer from your existing data
    * User experience reasearch
    * Survey
    * Focus groups



Gather Additional Data

* User Experience Research(UER)
  * Can go deep
  * \# participants is small
  * Good for brainstorming
  * Can use special equipment(like eye tracking)
  * Need to validate the results using retrospective analyses
* Focus groups: bring potential users for a group discussion
  * Not as deep as UER
  * Can bring participants than UER
  * Get feedback on hypotheticals
  * If you have a skilled facilitator, you run the risk of group think and convergence on fewer opinions
* Surveys 
  * Not as deep as Focus groups
  * More participants than focus groups
  * Useful for metrics you can not directly measure
  * Can't directly compare to other results because users might not tell the truth and **population** for your survey might be different from other internal metrics



Examples of applying metrics

* Measure user engagement
  * Survey responses as proxy
  * UER to brainstorm
  * Retrospective analysis to see user behavior
* Decide whether to extend inventory
  * Focus groups to get ideas from users
  * External data to see what uses want on other shopping site
* Which ads get most views
  * External data
  * UER: Eye-tracking machine
  * Retrospective analysis





## 4. Metrics Definition and Capturing data

### Definition

* Fully define what data are we going to look at to actually compute the metric
* Given those events/data, how do I summarize my metric



### Build Intuition

E.g. Click-through rate > 10% is not realistic



### Capturing data

* The technology used for counting clicks in different browsers(e.g. IE, Safari) might be different. So the difference of CTR in different platform might not be due to the users but the underlying technoloigy



### Example: Defining a metric

High level metric: Click-through probability =\# users who click / \# users who visit

Detailed metric:

* Def 1 (unique): For each \<time interval\> ,   = \# cookies that click / \# cookies
* Def 2(unique) : = \# pageviews results in a click within \<time interval\> / \# pageviews
  * This data capture is usually easier than recording cookies and then later grouping by cookies
* Def 3: = \# clicks / \# pageviews (click-through rate)



### Which metrics have which problems

check the metrics that will stay the same under the following conditions

|                                                            | Def 1: Cookie prob | Def 2: Pageview prob | Def 3: Rate |
| :--------------------------------------------------------: | :----------------: | :------------------: | :---------: |
|                Double Click or Single Click                |         *          |          *           |             |
| Back button or  caches page(not generate second page view) |         *          |                      |             |
|              Click-tracking bug of Javascript              |                    |                      |             |

Note: For the click-tracking bug of missing clicks, all three metrics will be affected.





## 5. Filter and Segmenting data

* External Reason
  * Spam and fraud from competitors messing up your metrics
* Internal Reason
  * Your change only impacts a subset of your traffic (e.g. you don't want to internalize your change but just within English traffic)



### How to use the filters

* Compute a baseline value for your metric
* To figure out whether or not you are biasing your data
  * Slice your data, compute the metric on different disjoint sets



Filtering and segmenting data is usually good for evaluating definitions and building intuitions.

* You can look at how the different definitions vary by segments

Example:

* Metrics: Total active cookies over time
* Build cookies week-over-week plot: \# cookies now / \# cookies a week ago, which can smooth out the weekly variation
* Build year-over-year plot: if abnormal spike still exists, we dive into different segments of the population to see if one segment is causing the spike





## 6. Summary Metrics

* Categories of metrics
  - Sums and Counts
    - \# users who visited page
  - Distribution metrics: mean, median and percentiles
    - Mean age of users who completed a course
    - Median latency of page load
  - Probability and rates
    - Probability has 0 or 1 outcome in each case
    - Rate(two counts divided by each other) has 0 or more
  - Ratios(two numbers divided by each other) 
    - P(revenue-generating click) / P(any click)



* **Sensitivity**
  * Detect a change when you're testing your possible future options
  * Characterize the distribution
    * Do the retrospective analysis and plot the histogram
    * If it looks normal, mean and median is good
    * If one-sided or lopsided,  maybe try 25%, 75% or other percentile
  * * 
* **Robustness** 
  * Metric stays the same when changes happen but you don't care about
  * How to check?
    * Run new experiments
    * Look at the logs or history metrics



### Example: Measuring sensitivity and robustness

choose summary metric for latency of a video

* Draw density line for similar videos to make sure they are comparable
* Draw the summary metric, when checking robustness, they should stay the same under these videos

* Draw the summary metric, when checking sensitivity, they should be able to detect the trend



* **Variability**

To calculate a confidence interval, you need:

* Variance
* Distribution

|  Type of metric   |                       Distribution                       |      Estimated Variance       |
| :---------------: | :------------------------------------------------------: | :---------------------------: |
|    Probability    |                         Binomial                         | $\frac{\hat p (1-\hat p)}{N}$ |
|       mean        |                          Normal                          |   $\frac{\hat\sigma^2}{N}$    |
| median/percentile |                     depends on data                      |               d               |
| Count/difference  |                      Normal(maybe)                       |          Var(X)+Var           |
|       Rates       |                         Poisson                          |           $\bar x$            |
|      Ratios       | Depends on the distribution of nominator and denominator |            Depends            |



### Nonparametric Answers

* Easy to implement
* Computationally intensive
* E.g. Sign test



* **Empirical Variability**
  * **A versus A experiments**: Two control groups
    * If we observe too much variability in a metric in this experiment, the metric might be too sensitive
    * When to used
      * Compare results to what you expect (sanity check)
      * Estimate variance and calculate confidence
      * Directly estimate confidence interval
  * Bootstrap method
    * If the bootstrap estimate aligns with  the analytical estimate, move on
    * If not, run A/A test to check
  * Calculate a confidence interval empirically
    * For each experiment, calculate the difference in click-through probability between the two groups
    * Then use average of difference as point estimate and we can do it in two ways
      * Calculate the standard deviation of the differences and assume metric is **normally distributed**
      * Calculate an empirical confidence interval, making no assumptions about the distribution 
        * For a sample with 40 data points, the 95% CI should be [second smallest difference, second largest difference]