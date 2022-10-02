---
layout: post
title:  "Case Study 1 - Bike Share Ride"
date:   2022-09-25 10:20:19 -0400
categories: jekyll update
---
Followed following data analysis steps:

* Ask
* Prepare
* Process
* Analyze
* Share
* Act

**Scenario**

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director
of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore,
your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights,
your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives
must approve your recommendations, so they must be backed up with compelling data insights and professional data
visualizations.

**ASK**

Three questions will guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

Lily Moreno, The director of marketing and my manager has assigned the first question to answer: How do annual members and casual riders use Cyclistic bikes
differently?

**Key tasks**

1. Identify the business task
   * The main objective is to convert casual riders into annual members by exploring how differently they are using cyclists bikes.

2. Consider key stakeholders
   * The director of marketing(Lily Moreno), Cyclistic Marketing Analytics team, Cyclistic Executive team

**Deliverable**

* Find the differences between Casual Riders and Member Riders.

**PREPARE**

I will use Cyclistic’s historical trip data to analyze and identify trends.The data has been made available by
Motivate International Inc. under this [License](https://www.divvybikes.com/data-license-agreement). Datasets are available [here](https://divvy-tripdata.s3.amazonaws.com/index.html).

**Key tasks**

1. Download data and store it appropriately
 * Data has been downloaded and copies have been stored securely on Big query.

2. Identify how it’s organized.
 * The data is in .CSV (comma-separated values) format, and there are a total of 13 columns.

3. Sort and filter the data.
 * I will use the last 12 months(August 2021-July 2022) of the data for the analysis as it is more current.

4. Determine the credibility of the data.
 * The data is credible and appropriate for this Case Study.

**Deliverable**

Datasets provided [here](https://divvy-tripdata.s3.amazonaws.com/index.html) are used for this case study and loaded into Big Query. The data for each month was loaded in separate tables and then merged into a single table to present a unified yearly set of data for analysis.

{% highlight sql %}
CREATE TABLE`my-project-1988-351517.bike_share_data.bike_trip` AS
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202108`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202109`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_share_202110`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202111`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202112`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202201`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202202`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202203`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202204`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202205`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202206`
UNION ALL
SELECT  *, TIMESTAMP_DIFF(ended_at, started_at, SECOND) as elapsed_minutes,CAST(started_at AS STRING FORMAT 'DAY') AS WEEKDAY
FROM `my-project-1988-351517.bike_share_data.bike_trip_202207`
{% endhighlight %}


**PROCESS**

Cleaning and Preparing data for analysis

**Key tasks**

1. I decided to use Big Query for cleaning and preparing the data.

2. Remove unwanted data.
   * While loading data into Big Query table, removed the header row.

   ![CSV Skip Header](/docs/assets/images/image1.png){:class="img-responsive"}
3. Add calculated fields `elapsed_minutes` and `weekday`.
   * `TIMESTAMP_DIFF(ended_at, started_at, SECOND) AS elapsed_minutes`
   * `CAST(started_at AS STRING FORMAT 'DAY') AS weekday`    
<br>          
4. Verify that rider type is valid.
{% highlight sql %}
SELECT *
FROM `my-project-1988-351517.bike_share_data.bike_trip`
WHERE member_casual='NA' OR member_casual= '' OR member_casual IS NULL
{% endhighlight %}


**Deliverable**

The process of cleaning and preparing was documented above.


**ANALYZE**

Now that data is stored appropriately and prepared for analysis, it's time to analyze it.

**Key taks**

1. Aggregate your data so it’s useful and accessible.
2. Organize and format your data.
3. Perform calculations.
4. Identify trends and relationships.

**Deliverable**

1. A summary of the analysis.

* Analysis on 'ride_length'

Following query has been used to calculate the average ride length of member and casual rider 

{% highlight sql %}
Select
avg(elapsed_minutes) AS average_of_ride_length,member_casual
FROM `my-project-1988-351517.bike_share_data.bike_trip`
group by
member_casual
{% endhighlight %}

**Result**

| average_of_ride_length | member_casual |
| --- | ----------- |
| 1752.6753728650715 | Casual |
| 775.921758373272 | Member |


![Result](/docs/assets/images/image2.png){:class="img-responsive"}

From the Pie chart above, it can be clearly seen that Casual riders processing 69.30% and Members processing 30.70% of the dataset. So, in the last 12 months casual riders used ride share 40% more than the members.

* Analysis on 'ride_length' of member and casual rider on day of week

Following query has been used for it

{% highlight sql %}
Select
avg(elapsed_minutes) AS average_of_ride_length,member_casual,WEEKDAY as day_of_week
FROM `my-project-1988-351517.bike_share_data.bike_trip`
group by
member_casual,WEEKDAY
{% endhighlight %}

**Result**

| day_of_week	| casual	| member	| Grand Total|
| --- | ----------- | --- | ----------- |
| FRIDAY	| 1644.382848	| 756.3438737	| 2400.726722| 
| MONDAY	| 1783.274161	| 754.0412348	| 2537.315396|
| SATURDAY 	| 1910.052093	| 868.4415268	| 2778.49362|
| SUNDAY	| 2037.828046	| 877.6696573	| 2915.497703|
| THURSDAY	| 1571.759406	| 744.3787668	| 2316.138173|
| TUESDAY	| 1526.945871	| 728.7309161	| 2255.676787|
| WEDNESDAY	| 1500.088352	| 730.3327861	| 2230.421138|

![Result](/docs/assets/images/image3.png){:class="img-responsive"}

In the above Bar Graph we can see that member's average ride length is consistent throughout the week but going down on weekends but for the Casual riders it is less on weekdays and going up on weekends which is indicating that members are using bike share for commuting to fixed distances like work or school and Casual riders are using for leisure.


* Analysis on number of trips and avg ride length on day of the week

Following query has been used for it

{% highlight sql %}
Select
avg(elapsed_minutes) AS average_of_ride_length,member_casual,WEEKDAY as day_of_week,count(ride_id) as number_of_trips
FROM `my-project-1988-351517.bike_share_data.bike_trip`
group by
member_casual,WEEKDAY
{% endhighlight %}

**Result**

| SUM of number_of_trips	| member_casual|		
| day_of_week	| casual	| member	| Grand Total|
| --- | ----------- | --- | ----------- |
| FRIDAY	| 347642	| 466680	| 814322|
| MONDAY	| 299656	| 472392	| 772048|
| SATURDAY	| 527575	| 453490	| 981065|
| SUNDAY	| 475626	| 417978	| 893604|
| THURSDAY	| 316118	| 522662	| 838780|
| TUESDAY	| 273826	| 523387	| 797213|
| WEDNESDAY	| 281783	| 522648	| 804431|
| Grand Total	| 2522226	| 3379237	| 5901463|


![Result](/docs/assets/images/image4.png){:class="img-responsive"}


In the above Bar Graph we can see that member's average total ride is consistent throughout the week but going down on weekends but for the Casual riders it is less on weekdays and going up on weekends which is indicating that members are using bike share for commuting to fixed distances like work or school and Casual riders are using for leisure. 

The above graph also shows that even though the average ride length of casual riders is higher than the members, their number of rides are still lower than the member's which means there's more members than the casual riders.


* Analysis on average ride length of member and casual rider in all 4 seasons

Following query has been used for it

{% highlight sql %}
Select
avg(elapsed_minutes) AS average_of_ride_length,member_casual, month
from (
Select
elapsed_minutes,member_casual, cast(started_at as string format 'MONTH') as month
-- ,WEEKDAY as day_of_week,count(ride_id) as number_of_trips
FROM `my-project-1988-351517.bike_share_data.bike_trip`)
group by
member_casual,month


Select
avg(elapsed_minutes) AS average_of_ride_length,member_casual,
CASE
WHEN month in ('MARCH', 'APRIL', 'MAY') THEN 'SPRING'
WHEN month in ('JUNE', 'JULY', 'AUGUST') THEN 'SUMMER'
WHEN month in ('SEPTEMBER', 'OCTOBER', 'NOVEMBER') THEN 'FALL'
WHEN month in ('DECEMBER', 'JANUARY', 'FEBRUARY') THEN 'WINTER'
ELSE 'rest of the year'
END as season
from (
Select
elapsed_minutes,member_casual, cast(started_at as string format 'MONTH') as month
-- ,WEEKDAY as day_of_week,count(ride_id) as number_of_trips
FROM `my-project-1988-351517.bike_share_data.bike_trip`)
group by
member_casual,CASE WHEN month in ('MARCH', 'APRIL', 'MAY') THEN 'SPRING' WHEN month in ('JUNE', 'JULY', 'AUGUST') THEN 'SUMMER'
WHEN month in ('SEPTEMBER', 'OCTOBER', 'NOVEMBER') THEN 'FALL'
WHEN month in ('DECEMBER', 'JANUARY', 'FEBRUARY') THEN 'WINTER'
ELSE 'rest of the year'
END
{% endhighlight %}


**Result**

| SUM of average_of_ride_length	| member_casual|		
| season	| casual	| member	| Grand Total|
| --- | ----------- | --- | ----------- |
| FALL	| 1645.68049	| 760.7448901	| 2406.42538|
| SPRING	| 1850.78762	| 746.6216489	| 2597.409269|
| SUMMER	| 1798.985904	| 836.0944374	| 2635.080341|
| WINTER	| 1517.05427	| 680.6160926	| 2197.670363|
| Grand Total	| 6812.508284	| 3024.077069	| 9836.585353|

![Result](/docs/assets/images/image5.png){:class="img-responsive"}

From the Bar graph above we can see that bike share usage (both for members and casual riders) is highest in summers and lowest in winters and almost same in falls and spring.

**SHARE**

This phase will be done by presentation


**Key tasks**

1. Determine the best way to share your findings.
2. Create effective data visualizations.
3. Present your findings.
4. Ensure your work is accessible.

**Deliverable**

Supporting visualizations and key findings.

**Important Conclusions**

* Casual riders holds the biggest proportion of the total rides, 40% more than the members.

* Throughout all the 12 months casual riders are more than the members.

* Casual riders are using bike share more on weekends.

There is an indication that members use bikes for commuting to work but Casual riders are using for other purposes.


**ACT**

This phase will be done by the Cyclistic's executive team, Director of Marketing (Lily Moreno), Marketing Analytics team on the basis of my analysis. (Data-driven decision making)

**Deliverable**

* Your top three recommendations based on your analysis
 
 * Marketing campaigns targeting casual riders showing how membership can be cost effective for them .
 * sending coupons via emails and mails especially for winters as usage is lowest during this season.
 * offering some perks to members on using bikes on weekends.




---------


**To Be Removed**

You’ll find this post in your `_posts` directory. Go ahead an*d edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
