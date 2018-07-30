# Project Design Writeup and Approval Template

Follow this as a guide to completing the project design writeup. The questions for each section are merely there to suggest what the baseline should cover; be sure to use detail as it will make the project much easier to approach as the class moves on.

### Project Problem and Hypothesis
* What's the project about? What problem are you solving?
Analys of the relationship between running training and running race performance. Specifically we will analyse 12 weeks of GPS running data for runners in the lead up to the Athletics Victoria Albert Park 10km road race held on the 22nd of July 2018. Factors that will be analysed include training pace, training volume, number of runs per week, elevation and long run distance. The project will be successful if it can determine which factors best correlate to race performance and if we can build an accurate 10km race time predictor model.

Hypothesis: An increase in training pace, training volume or elevation will result in a better race performance. However to a certain point, too much of one of these factors could result in over training and a poor performance.

* Where does this seem to reside as a machine learning problem? Are you predicting some continuous number, or predicting a binary value?
For this project we will return a continuous number, the model would return a predicted 10km time or pace value. We would also return the strength of the relationship between training factors and race performance.

* What kind of impact do you think it could have?
The results could influence coaches and runners on how to best program training for a 10km race. Additionally it could also highlight if a particular runner is doing something bad or really well in their training compared to others, which we could inform them of.

* What do you think will have the most impact in predicting the value you are interested in solving for?
I am predicting that training volume per week will be the most influential factor.


### Datasets
* Description of data set available, at the field level (see table)
Running data is recorded from a runner's GPS device, most commonly a Garmin GPS watch. This data can be exported to csv format from the Garmin Connect website (by the individual runner). Alternatively running data can be retrieved manually from a social running website Strava. However this is a manual and time consuming process to enter data from Strava into a spreadsheet.

The plan for obtaining data is to request runners to export their 12 weeks of training data from Garmin Connect and send the csv file to through a google form. This form also collects some data about the runner including their age, gender, training age (how many years have they been training), their race result, and how they felt they performed in the race (from 1 terrible, to 5 great). The Garmin Connect data will added to with some data manually retrieved from Strava. The data from strava does not contain as many fields as the data from Garmin, however it does have the key information we are after.

#### Table 1 - Running data
| Variable | Description | Type |
|---|---|---|
| AthleteID | Identifies which athlete the run is linked to | Integer |
| Activity Type | Type of activity, this will mainly be running, but can include other activities such as walking and swimming | text/categorical |
| Date | Date and time of the activity | Datetime |
| Favorite | Did the user favorite this actvitity | Boolean |
| Title | Title text of the activity, may be edited by the runner or default text | Text |
| Distance | Run distance in km | Float |
| Calories | Calories burnt | Integer |
| Time | Duration of the activity | Time |
| Avg HR | Average heart rate during the activity | Integer |
| Max HR | Maximum heart rate during the activigty | Integer |
| Aerobic TE | Training effect on the aerobic system. 0 = No benefit, 1 = Minor benefit, 2 = Maintaining, 3 = Improving, 4 = Highly improving, 5 = Overreaching | Float |
| Avg Run Cadence | Average number of steps per minute | Integer |
| Max Run Cadence | Maximum number of steps per minute | Integer |
| Avg Pace | Average running pace (speed) in minutes per kilometer | Time |
| Best Pace | Fastest speed during the activity in minuters per kilometer | Time |
| Elev Gain | Total elevation gain during the activity in meters | Integer |
| Elev Loss | Total elevation loss during the activity in meters | Integer |
| Avg Stride Length | Average stridge length (distance covered in one step) in meters | Float |
| Avg Vertical Ratio | Ratio of vertical distance travelled (up and down bounce) to horizontal distance covered | Float |
| Avg Vertical Oscillation | Average amount of vertical (up and down) movement in centimeters | Float |
| Avg Ground Contact Time | Average amount of time the foot is in contact with the ground in milliseconds | Integer |
| Avg GCT Balance | Average balance of ground contact time between left and right foot | Text |
| Avg Run Cadence | Appears to be a repeated field with no data, possibly in use for a different recording device | Null |
| Max Run Cadence | Appears to be a repeated field with no data, possibly in use for a different recording device | Null |
| Normalized Power | Power meter data, no applicable | Null |
| L/R Balance | Does not appear to be used | Null |
| Training Stress Score | Does not appear to be used | Integer |
| Max Avg Power (20 min) | Related to power meters, does not appear to be used | Null |
| Total strokes | Swimming metric, not used for running | Null |
| Avg. Swolf | Swimming metric, not used for running | Null |
| Avg Stroke Rate | Swimming metric, not used for running | Null |
| Max Depth | Diving metric, not used for running | Null |
| Bottom Time | Diving metric, not used for running | Null |
| Min Water Temp | Minimum temperature recorded by the watch | Integer |
| Gas Type | Diving metric, not used for running | Null |
| Surface Interval | Diving metric, not used for running | Null |
| Decompression | Diving metric, not used for running | Null |
| Weight | Does not appear to be used for running | Null |
| Current | Does not appear to be used | Null |
| Surface Conditions | Not used for running | Null |
| Water Type | Not used for running | Null |

#### Table 2 - Athlete data
| Variable | Description | Type |
|---|---|---|
| AthleteID | Unique identifier for the athlete, used to link athlete to running data | Integer |
| Email | Email address for contacting the athlete with results, to be kept private | Email |
| Name | Name for my reference and contact information, will not be used in the modelling | Text |
| Gender | Male or Female | Text |
| Age | Current age of the athlete | Integer |
| Training age | Number of years the athlete has been training for | Integer |
| Albert Park 10km Result | Finish time for the race | Time |
| Result self rating | Athlete's self rating of the result between 1 and 5, with 1 being Terrible and 5 being Great | Integer |

* If from an API, include a sample return (this is usually included in API documentation!) (if doing this in markdown, use the javacription code tag)


### Domain knowledge
* What experience do you already have around this area?
Very good knowledge of running data and terminology from experience with running and run coaching.

* Does it relate or help inform the project in any way?
Yes domain knowledge is helpful for an understanding of the different metrics available and how they may impact race time. It's also helpful for an understanding of data collection, i.e. knowing about Garmin watches and what data is available. Additionally knowing what training period to capture, i.e. 12 weeks is a fairly standard training block length which allows enough time for adaption and improvement from training.

* What other research efforts exist?
    * Use a quick Google search to see what approaches others have made, or talk with your colleagues if it is work related about previous attempts at similar problems.    
    * This could even just be something like "the marketing team put together a forecast in excel that doesn't do well."
    * Include a benchmark, how other models have performed, even if you are unsure what the metric means.
    
There is a lot of different and conflicting research available on training methodology for running. However it tends to be more of an art than a science, with many different methods that can achieve training adaptation, and different training methods can work for different people.

One study I found (https://www.physiology.org/doi/10.1152/japplphysiol.00801.2016) compared a group of runners training at lower mileage compared to runners training at a higher mileage. This study found the runners who trained at higher mileage were more efficient and were therefore able to run at faster paces with less effort compared to the low mileage runners. With this benchmark we would expect that runners with higher mileage training will have run a faster 10km race time.

### Project Concerns
* What questions do you have about your project? What are you not sure you quite yet understand? (The more honest you are about this, the easier your instructors can help).
I'm still contemplating how it's all going to come together. I expect I'll be grouping and averaging data to be able to compare runners. For example for mileage I might sum the kilometers per weekly period, and then take the average of that to get an average weekly mileage for each runner. That would allow an easier comparison. I also suspect that working with time data could be an issue, these may need to be converted to floats or integers.

* What are the assumptions and caveats to the problem?
    * What data do you not have access to but wish you had?
    * What is already implied about the observations in your data set? For example, if your primary data set is twitter data, it may not be representative of the whole sample (say, predicting who would win an election)
    
Collecting the data is my biggest concern. I have put out a social media post and emails to a group of runners who competed in the event, and hope to be able to get a decent sample size. Up to 20 samples would be a good figure. I may have to supplement this with the manual process of recording data from Strava which I have started but is time consuming.
    
* What are the risks to the project?
    * What's the cost of your model being wrong? (What's the benefit of your model being right?)
    * Is any of the data incorrect? Could it be incorrect?
    
There are many factors that can impact running performance. To list a few examples this can include not enough sleep, poor nutrition, injuries, just a bad day, etc. Additionally some runs are interval sessions which may effect the modelling, for example a runner can complete a fast interval, stop for recovery, repeat the next interval - as far as the running data is concerned this could look like one fast run instead of a number of short runs with rest inbetween. So there are a few factors that may impact the accuracy of the model.

### Outcomes
* What do you expect the output to look like?

I'm expecting to have a visual output to show the relationship of the training factors to race result. Ideally I'll have some way to clearly show which factors have the strongest relationship and which are weakest. Linear models plotted?

* What does your target audience expect the output to look like?

The target audience, coaches and runners, will be expecting simple and clear information that they can take away and apply to their training. It may be a simple chart that clearly shows that one training factor has the strongest relationship with race result, therefore focussing on that factor could potentially have the biggest impact on improving 10km race times.

* What gain do you expect from your most important feature on its own?

Expect that showing the strongest relationship on it's own would be useful information for coaches and runners to inform training programs, whether that means changing anything or confirming their existing methods.

* How complicated does your model have to be?

Not very complicated, I think trying linear regression models with various factors would be appropriate.
There will be a reasonable amount of work required with data cleansing, removing unnecessary columns, handling of missing data some values are blank and some are stored as "--", converting time data to decimal to be more useful to work with, and translating back again, grouping of data into weeks and averaging across weeks, addressing outliers such as a runner who has bounced back from injury could have been on quite low volume but performed well on race day.

* How successful does your project have to be in order to be considered a "success"?

If I can get a reasonable number of samples (let's say 10 minimum but hopefully 20 or more) and able to identify one or more correlations between training data and race result that would be a success.

* What will you do if the project is a bust (this happens! but it shouldn't here)?

I don't believe the project will be a bust, there is a risk with data collection however the worst case if I am really struggling to get runners to send me their data I can manually retrieve data from Strava. Which I have commenced it's just time consuming. If I really can't get anything useful out of this project I could fall back to another idea that I presented with cooling tower non-compliances relationship with legionnaires disease detections. 

