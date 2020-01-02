# AB Testing - Lesson 4 - Design An Experiment

* Choose "subject"
* Choose "population"
* Size
* Duration





## 1. Unit of Diversion

How to define a individual subject in the experiment?

* User ID (used for login)
  * Stable, unchanging (whether it's from website or app)
  * Personally identifiable
* Anonymous ID (cookie): some identifier randomly generated when logging in
  * Changes when you switch browser or device
  * Users can clear cookies
* Event: On every single event, you redecide whether that event is in the experiment or in the control
  * No consistent experience
  * Only appropriate when the changes are not user visible(e.g. change of a ranked list)



Less common options

* Device ID
  * Only available for mobile
  * Tied to specific device
  * Unchangeable by user
  * Personally identifiable
* IP address 
  * Changes when location changes



### Example

In the following table, "1" means if the user use the unit of diversion in the first column for the following events, they might be switched from experiment to control or vice versa. Blank means they will stay in the same group. "?" means not sure.

|            |                 Desktop Homepage                 |          Sign In          |  Visit Class   | Watch Video(Desktop) | Mobile(Auto Sign In) | Watch Video(mobile) |
| :--------: | :----------------------------------------------: | :-----------------------: | :------------: | :------------------: | :------------------: | :-----------------: |
|  user-id   | Can't assign user to a group before they sign in |             1             |                |                      |                      |                     |
|   cookie   |                        1                         | ?(could clear the cookie) |       ?        |          ?           |          1           |          ?          |
|   event    |                        1                         |             1             |       1        |          1           |          1           |          1          |
| device-id  |                  Not applicable                  |      Not applicable       | Not applicable |    Not applicable    |          1           |                     |
| IP Address |                        1                         |             ?             |       ?        |          ?           |          ?           |          ?          |







## 2. Consistency of Diversion

User-id: Users get consistent experience as long as they're signed in.

* E.g. how courses are being displayed

Cookies: Users get consistent experience as long as they use the same device

* Test the change across the sign-in and sign-out border. Change the layout of a page like the location of the sign in bar

IP address

* Change the host provider



### Example

Which unit of diversion will give enough consistency?

|              Experiment               | Event | Cookie | User-id |
| :-----------------------------------: | :---: | :----: | :-----: |
|    Change reducing video load time    |   1   |        |         |
|     Change button color and size      |       |   1    |         |
|    Change order of search results     |       |        |    1    |
| Add Instructor's notes before quizzes |   1   |        |         |

Note: 

* When user might not notice the change, we tend to use the event-based diversion



## 3. Ethics of Diversion

If using user id, we know that the data is identified

* Need informed consent

If using cookies,

* Might not  need informed consent



### Example

Which experiments might require additional ethical review?

* News letter prompt after starting course (user id diversion): No
  * No new information being collected 
  * Fine if original data collection was approved
* Newsletter prompt on course overview (cookie diversion): Yes
  * Depends: Are email addresses stored by cookie
  * Potentially impacts other data collection
  * The cookie is linked to the email address
* Changes courses overview page( cookie diversion)
  * Not a problem





## 4. Variability

The variability calculated empirically might be usually larger than the one calculated analytically because the **unit of analysis** is different from the unit of diversion.



### Unit of Analysis

Whatever the denominator of the metric is, 

*  If same as the unit of diversion(e.g. event-based diversion), then empirical result will be close to analytical result
* If not the same(e.g. the diversion is user-id or cookie), then these two results will be different



**Reasons of the difference**

* When diverted by event, we assume that each data we draw is independent
* When diverted by user-id, the data are divided by group and are correlated



So the conclusion is 

* When unit of analysis = unit of diversion, variability tends to be lower and closer tot analytical estimate





## 5. Choosing Population

* In A/B testing, we talk about the inter-user experiment
  * We got different people on the A side and B side

* If we think we think identify what population will be affected by the experiment, we might want target the experiment to that traffic
* Changing the population can also affect the variability as well



### Cohort

People who enter the experiment at the same time. We only look at the initial groups.

* Cohort is harder to analyze than population. They're going to take more data because you'll lose users
* When to use cohort
  * Looking for learning effects
  * Examining user retention
  * Want to increase user activity
  * Anything requiring user to be established 



**Example**: Audacity

Have existing course and change structure of lesson

Unit of diversion: user-id - but, can't run on all users in course 





## 6. Sizing

### How variability affect sizing

**Example 1**: Audacity includes promotions for coaching next to videos

Experiment: Change wording of message

Metric: Click-through rate = \# clicks / \# pageviews

Unit of diversion: pageview or cookie

Analytic variability won't change, but probably under estimate for cookie diversion

Empirical estimate with 5000 page views

​	By pageview: 0.00515

​	By cookie: 0.0119



To calculate size , assume SE ~ 1/sqrt(N)

If the practical significance boundary is $d_{min}=0.02$,

* Diverting by pageview needs: 2600 samples
* Diverting by cookie needs: 13900 samples





**Example 2**: Audacity changes order of courses on course list

Metric: click-through rate

Unit of diversion: cookie

Which strategies could reduce the number of pageview?

* Increase $d_{min}$, $\alpha$, $\beta$
* Change unit of diversion to page view
  * Makes unit of diversion same as unit of analysis to decrease the variability
  * But will less consistent experience be ok?
* Target experiment to specific traffic
  * Non-English traffic will dilute the results
  * Could impact choice of practical significance boundary
* Change metric to cookie-based click-through probability
  * Often doesn't make significant difference 
  * If there is a difference, variability would probably go down





### Sizing Triggering

Run a pilot to see whom in the population are being affected by your change



## 7. Duration

* What's the duration of the experiment to run
* When do I want to run the experiment
* What fraction of the traffic you are going to send through your experiment



### Duration vs. Exposure

**Example**

Size of an experiment: 1 million pageviews

Average traffic per day: 500,000 pageviews 

* First thought: We can run experiment for 2 days 
* But we might have weekly variation in traffic and metric
  * We can run on mix of weekend and weekday days
* For risky change, run longer with less traffic 



### When to limit exposure

Which experiments are risky enough that Audacity might want to limit the number of users exposed?

* Changes database : Yes, if this goes wrong, effects could be huge
* Change color of "Start now" button (low risk)
* Allows Facebook login
*  Changes order of courses on course list





## 8. Learning Effects

When you want to measure user learning or whether user has adapted to a change or not

Things to keep in mind for measuring user learning:

* Choose the unit of diversion correctly
* Dosage: How often do user see the change, probably use cohort rather than population
* Risk and duration
* Pre-periods(uniformity trials)
  * A/A tests to find system variability, user variability
  * Make sure that we don't have any difference in the population. The difference is due to experiment
* Post-periods(uniformity trials)
  * A/A tests after A/B test to attribute the difference to user learning that happen in the experiment period













 