# AB Testing - Lesson 1 - Introduction



## 1. Overview

### What

A general methodology used online when you want to test a new product or a new feature.



### How

Take two sets of users,

* Control set, your existing product 
* Another set, your experiment, the new version

How do these two sets  respond differently?



### Useful when

* You want to climb to the peak of the current mountain 

eg.

* Movie recommendation site
* Change backend-page load time, results



### Not useful when

* not when you choose which mountain to climb(i.e. testing out new experiences)

* Short term A-B testing
* Can't tell if you're missing something

eg.

* Online shopping service. Is item complete?
* New premium service
* To long to get repeat customers
* Update brand and logo





## 2. Example

**Audacity**

* Create online finance courses
* User flow / Customer funnel
  * Homepage visits
  * Exploring the site
  * Create account
  * Complelte
* Experiment
  * **Hypothesis**: Changing the "start now" button from orange to pink will increase how many students explore the Audacity's courses





## 3. Choose a metric

Following the audacity example,



* Total number of courses completed
  * Too much time to accumulate, not practical
* Number of clicks  on specific button
  * The number of total clicks in old and new version is different
* **CTR(Click Through Rate)** Number of clicks on specific button / Number of page views
  * Maybe the page loads slowly and the visitor impatiently clicks 5 times
  * Use when deciding whether user progresses to the second level of the funnel: Homepage visits
* **Click through probability**: Unique visitors who click / Unique visitors who view the page
  * Use when deciding whether user progresses to the second level of the funnel: Exploring the site



Update hypothesis: Changing the "start now" button from orange to pink will increase the **Click Through Probability** of the button



## 4. Review statistics

### Which distribution?

* Binomial distribution
  * Click: success   No click: failure
  * Mean: p
  * Std: $\sqrt{\frac{p(1-p)}{N}}​$
  * $\hat p=\frac{X}{N}$
  * When  we can use?
    * 2 types of outcomes
    * Independent events
      * Clicks on a search result page is **not independent** because people will search again with slightly different word if they don't find the answer in the first search
      * Student completion of course after 2 months: can be assumed independent (exception: student creates 2 accounts; take courses with friends)
    * Identical distributions



### Confidence Interval

If we theoretically repeat the experiment over and over again, we would expect the interval we construct around our sample mean to cover the true value in the population 95% of the time



* To use normal, check $N\cdot \hat p > 5$ and $N\cdot (1-\hat p) > 5$
* Margin: $m=z\cdot\sqrt{\frac{\hat p(1-\hat p)}{N}} $
  * Large $\hat p$: Std will be smaller, distribution will be tighter, confidence interval will be smaller
  * Large $N$: Std will be smaller, distribution will be tighter, confidence interval will be smaller
  * When considering 95% confidence interval, z-score is 1.96



### Hypothesis Testing

We want to calculate p(results due to change). We have two distribution: $p_{control}$ and $p_{exp}$(experiment)

* Null hypothesis: $p_{control} = p_{exp}$
* Alternative hypothesis: $p_{control} \neq p_{exp}$
* Measure $\hat p_{control}$ and $\hat p_{exp}$ and calculate $p(\hat p_{exp}-\hat p_{control}|H_0)$ and reject null if the previous p is below 0.05



#### Compare two samples

The quantitative task tells us whether it's likely that the difference we observed could have occurred by chance, or if it would be extremely unlikely to occur if the two sides are actually the same



* Variables we have: $X_{control},X_{exp},N_{control},N_{exp}$
* $\hat p_{pool}=\frac{X_{control}+X_{exp}}{N_{control}+N_{exp}}$
* $SE_{pool}=\sqrt{\hat p_{pool}(1-\hat p_{pool})\cdot(\frac{1}{N_{control}}+\frac{1}{N_{exp}})}$
* $\hat d=\hat p_{exp}-\hat p_{control}$
* $H_0:d=0$, $\hat d\sim N(0,SE_{pool})$
* If $\hat d>1.96\cdot SE_{pool}$ or  $\hat d<-1.96\cdot SE_{pool}$, reject null



Make sure that the experiment is practically significant(from business viewpoint, e.g., 2% change in the click through probability ) and statistically significant. **Set the statistical significance bar lower than the practical significance bar**.





## 5. Design

### How many page views we need?

* $\alpha=p(reject\, \,null|null\,\,true)$
* $\beta=p(fail\,\,to\,reject|null\,\,false)​$
* Small sample
  * Low $\alpha$: you are unlikely to launch a bad experiment
  * High $\beta$: you are likely to fail to launch an experiment that actually did have a difference you care about
* Large sample
  * Same $\alpha$
  * Low $\beta$

* Sensitivity=$1-\beta$, often $80\%$



Relationship between change and sample size

|                      Change                       |                      Sample size change                      |
| :-----------------------------------------------: | :----------------------------------------------------------: |
|        Higher CTP in control(still < 0.5)         |  Increase page views(reduce the std to  the original level)  |
| Increased practical significance level($d_{min}$) |   Decrease page views(Lager changes are easier to detect)    |
|      Increased confidence level($1-\alpha$)       | Increase page views(Reject the null less often but we need to keep the sensitivity the same) |
|          Higher sensitivity $(1-\beta)$           |         Increase page views(Narrow the distribution)         |

 



## 6. Analyze

* Confidence interval larger than $d_{min}$: launch the new version
* Confidence interval falls within $[-d_{min},d_{min}]​$: not launch because it's not practically significant
* Confidence interval overlaps with $[-d_{min},d_{min}]$: run additional test. When you don't have time to do this, talk to the decision-makers and use other methods beside data



