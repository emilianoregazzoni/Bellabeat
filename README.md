#  Bellabeat
## __Bellabeat Data Analysis Project__  

About the company:  
  
Bellabeat is a real company, it was founded by Urška Sršen and Sando Mur, a high-tech company that manufactures health-focused smart products. Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.  
By 2016, Bellabeat had opened offices around the world and launched multiple products. Bellabeat products became available through a growing number of online retailers in addition to their own e-commerce channel on their website. The company has invested in traditional advertising media, such as radio, out-of-home billboards, print, and television, but focuses on digital marketing extensively. Bellabeat invests year-round in Google Search, maintaining active Facebook and Instagram pages, and consistently engages consumers on Twitter. Additionally, Bellabeat runs video ads on Youtube and display ads on the Google Display Network to support campaigns around key marketing dates

Scenario:   
  
I am a junior data analyst working on the marketing analyst team at Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. You have been asked to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The insights you discover will then help guide marketing strategy for the company. You will present your analysis to the Bellabeat executive team along with your high-level recommendations for Bellabeat’s marketing strategy.  
I should prepare the data, process it, analyze it and get some insights to give them for stakeholders, and help to bring the company on to the next level.


:scroll: The 6 stages from Google Data Analyst program are:  
Ask - Prepare - Process - Analyze - Share - Act  


## __Ask__  
Business task: 
Process, analyze, create and share visualizations about FitBit dataset, to get patterns and trends which can lead for new business opportunities and could help to Bellabeat to grow up.  
The stakeholders are the founders of the company Urška Sršen and Sando Mur, also the Data analyst marketing team will be beneficiated for the research.

## __Process__  
I have used use R studio desktop versión 2022.07.0 Build 548 for analyzing the .csv files.

__R programming process__

First steps, install all the packages and load all the libraries necessary

``` 
install.package('readr')
install.packages('dplyr')
install.packages('tidyverse')
install.packages('here')
install.packages('janitor')
install.packages('stringr')
install.packages('lubridate')
install.packages('chron')
install.packages('ggplot2')

library(dplyr)
library(tidyverse)
library(here)
library(skimr)
library(janitor)
library(stringr)
library(lubridate)
library(ggplot2)

``` 
Import and clean the files: load the .csv files. Remove duplicates and fix inconsistencies.

```
sleepDay <- read_csv("C:/Fitabase Data 4.12.16-5.12.16/sleepDay_.csv")
View(sleepDay)
dailyActivity <- read_csv("C:/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
View(dailyActivity)
weightLogInfo <- read_csv("C:/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
View(weightLogInfo)
dailyCalories <- read_csv("C:/dailyCalories_merged.csv")
View(dailyCalories)
```

Start checking all the data, for example Daily activities

```
sum(is.na(sleepDay)) : 413 records
sum(duplicated(sleepDay)): 3 records
```

So, I should remove 3 records, best way to do that:
```sleepDat <- unique(sleepDay)```


Output: 410 records which is razonable.

__Repeat the same steps for all tables you want to analyze.__  

After all that, there is no repeated information in the tables.
Now let’s check it out visually.
The column which contains the date is in Character format →  

![image](https://user-images.githubusercontent.com/20979227/181067618-87a159a8-0bb2-4b18-ac41-f720c5e68718.png)

It must be changed →   
But, ‘AM’ last part, is the tricky part. I can’t tell R this field is a date, with ‘AM’.
All the registers have the ‘AM’ termination, I can delete it.
So first, make a transformation to delete the last two characters of the field, in this case ‘AM’ will be dropped.
Now I got the value in date format.

```sleepDay$Date <- mdy_hms(gsub('.{2}$', '', sleepDay$SleepDay))```

See how many records grouped by user has the data set

```
##Create filter
sleepByUser = sleepDay %>% group_by(Id)

##Show summarized
sleepByUser %>% summarise(
  Id = mean(Id),
  TotalMinutesAsleep = sum(TotalMinutesAsleep)
)
```
__SQL process__  

Despite the course recommending using BigQuery, it is not  my favorite tool to deal with so many records. At least on my computer, which is not a bad one, the website is so slow.
I prefer to use MySQL and create the database through the csv files, create the tables, and make the records with some scripts.
I can create the table and insert the records automatically from .CSV taking some help to build the scripts with the website https://sqlizer.io/  
All the scripts to build the database are in the repository.  

There are the 18 files associated with each .csv, created with https://sqlizer.io/. Sqlizer is a pay service if you want to insert so many records like some tables (2.48 million like heartrate_seconds). I took it for once.  
Feel free to use the scripts .SQL for your own analysis.  

I made a brief research about the table dailyactivity  
I do some filters in SQL, and see that all information about daily activities are between  
2016-04-12 00:00:00 to 2016-05-12 00:00:00

```SELECT * FROM dailyactivity d
order by ActivityDate;
```

It is impossible to filter, because I only have 1 month, so if I want to measure the activity, I only have the days to filter and try to get some classified information.

Create a view to have the data accesible:  

```create view DayWeek as
SELECT DAYNAME(ActivityDate) as DayWeek
from bellabeat.dailyactivity;  
```

```SELECT DayWeek FROM dayweek;```  

After looking for some time all the tables, there is 2 important tables:
__dailyactivity__ and __sleepday__.  
The table of dailyactivity has a lot of information about the users, like steps, total distance, and calories burned.
Date format is ok

![image](https://user-images.githubusercontent.com/20979227/181069611-737e41ac-71e1-47e1-89ef-c4c78e12092e.png)

## __Analyze/share phase (SQL and R)__

Curious searching, it is impossible to have a register which has more time asleep than more time in bed, so try to see what happens looking for this.  
No records found, what it is ok, more confident about data.  
Try to find a record of a people who felt asleep at the same minute the person got to bed.  

```  
timeSleepVsBed <- filter(sleepDay, TotalMinutesAsleep >= TotalTimeInBed)
View(timeSleepVsBed)  
```  

Just one.  
  
  
![image](https://user-images.githubusercontent.com/20979227/181069921-d03a56a4-f642-4658-9006-ff0d85ed3152.png)

Let’s see how many minutes are the difference between users getting to bed and falling asleep.  
For this, add a column to data frame with the difference:  

``` 
sleepDay$DiffBedSleep<- (sleepDay$TotalTimeInBed-sleepDay$TotalMinutesAsleep)
```  
![image](https://user-images.githubusercontent.com/20979227/181070233-0782e628-eeec-4aac-93c8-da5b2f9832d4.png)  


Now get the average time of all users, all time records.  

```
##Get the average all users, all time records
print(mean(sleepDay$DiffBedSleep))
```  
![image](https://user-images.githubusercontent.com/20979227/181071814-baae8b20-831c-4a3d-b456-cd8c3459c4b0.png)  
+39 minutes is the average to get asleep for the users.  
It seems to be a huge amount of minutes. World average is between 10 minutes and 20 minutes to fell asleep. It seems to be a problem. The average of the set of 30 person is the double of the World average.  
__This is an insight__

Now make a query, to obtain the sum of, (grouped by day week):  

```
SELECT DAYNAME(ActivityDate),
SUM(d.TotalSteps) as TotalSteps, ROUND(SUM(d.TotalDistance),2) as TotalDistance, SUM(d.calories) as TotalCalories
FROM dailyactivity_merged d , dayweek w
group by DAYNAME(ActivityDate)
order by d.ActivityDate  

```  

Now I have the information grouped, I will name it “Resume general”  
![image](https://user-images.githubusercontent.com/20979227/181138992-76cac447-b7a7-4bd0-afec-260b294ceb46.png)  

 

I have explored correlations in the data.  
Something obvious is the relation between steps and calories burned. As you walk in a day, you will increase your burned calories. 
Check it out:  
After explore the graphs ggplot2 give us, for work with 2 variables, I took geom_smooth  

``` 
df <- dailyActivity
ggplot(df, aes( df$Calories ,df$TotalSteps )) +
  geom_point() +
  geom_smooth(method = "lm")
 ```   
 
  ![image](https://user-images.githubusercontent.com/20979227/181072753-54f6a96c-7af4-4ef2-83a9-81e5d285d2a8.png)  
  
  
This graphic properly shows the correlation between the 2 variables used, steps and calories burned.  
It seems to be so good, just for one particular value.  
There is a record that indicates +3500 were burned but just with a bit more of +2500 steps.   
Inusual than the others, so let’s check that particular record.  
Get all data ordered by TotalSteps, the first one is that particulary point at the graph. Also this record has a very high value in VeryActiveDistance , which is completely ok, that means this event probably was an High Intensity Interval Training (HIIT). A kind of workout with a particular intensity that makes you spend a lot of calories, with no necessary walk so much.  


``` 
dailyActivity %>% arrange(-dailyActivity$TotalSteps)  
```  

![image](https://user-images.githubusercontent.com/20979227/181072989-43bc3b66-1173-430f-8455-adf6ad6c62a7.png)  


Searching on the internet, I found the average calories burned by a person rounds between 2000 to 2500.   
Let’s check if the average calories burned by users it’s ok, or maybe some users need to do more activity.  
Get the sum of the calories burned by user:  

``` 
CaloriesByUser <- aggregate(x = dailyActivity$Calories,            
          by = list(dailyActivity$Id),           
          FUN = sum) 
          
CaloriesByUser$RecordByUser <- count(dailyActivity, Id)
```  

![image](https://user-images.githubusercontent.com/20979227/181073231-a1264752-d57a-4c3c-b480-041df6602dfe.png)  

Now get the Id user and x value means that the amount of calories burned of its records.  
Column names are so bad, I change them by setting the correct Column name through vector position.  

```  
names(CaloriesByUser)[1] <- "IdUser"
names(CaloriesByUser)[2] <- "SumCalories"
```  

![image](https://user-images.githubusercontent.com/20979227/181073445-5ba06718-07d6-4abb-b530-cf8e8718729f.png)  

Now should get the occurrences of each user on the table DailyActivity, so →  

```  
CaloriesByUser["QtyUser"] <- select(count(dailyActivity, Id), n)  
```  

![image](https://user-images.githubusercontent.com/20979227/181073527-cda45dcd-9a55-4503-80da-614cfa9e43ee.png)  
  
  

Finally get the data frame, let’s check quickly if it is ok in SQL.  
Check the occurrences of the user = 4057192912  


``` 
SELECT * FROM dailycalories d
where Id = 4057192912;
```   
![image](https://user-images.githubusercontent.com/20979227/181073655-3fafde4f-7b6c-4668-a7ce-062e52bb709e.png)  

  
Just 4, what seems to be ok.  
Now create the column with the average for each user:  


```
CaloriesByUser <- CaloriesByUser %>% 
  mutate(SumCalories/QtyUser)
```

![image](https://user-images.githubusercontent.com/20979227/181073909-204fdb26-080b-4a12-b559-323ac6d93035.png)  


Rename the column name  

```
names(CaloriesByUser)[4] <- "AvgUser"
```  

![image](https://user-images.githubusercontent.com/20979227/181073979-ec7ea139-4935-475d-a8e5-c2180d5ae137.png)  

I Got the average.  
I found on the internet, a normal adult should bourn around 2000 calories daily, with the attribute AvgUser, create a column showing if the user burns 2000 calories or not.  
I will create a logic field, with this information.  

``` 
CaloriesByUser <- CaloriesByUser %>% mutate(CaloriesByUser$AvgUser > 2000)
names(CaloriesByUser)[5] <- "CaloriesOK"
``` 

![image](https://user-images.githubusercontent.com/20979227/181074232-8c51acbc-3960-49e2-9846-dbce9ff3c761.png)  

Now try to get a pie chart that shows that information about CaloriesOK.  

``` 
labels <- c("<2000 calories/day", ">2000 calories/day")
pie(table(CaloriesByUser$CaloriesOK),labels)
```  

![image](https://user-images.githubusercontent.com/20979227/181074362-083350eb-c82b-43ce-a46a-275a2ba755a4.png)  


Seems to be ok, when users turn on the device to save information about them, we have relatively good active days, mostly users burn the minimum amount they should burn in a day.  
Despite this, a big amount of users (around 1/3) do not burn enough calories. This is another insight.

Now, let’s check the hours,  with more steps produced by hour.  
Make a query:   

```  
SELECT hour(ActivityMinute), SUM(Steps)
FROM minutestepsnarrow m
group by hour(ActivityMinute)
order by Id,hour(ActivityMinute) ;
```
![image](https://user-images.githubusercontent.com/20979227/181074976-08395784-be89-4692-8600-29527ad9e96f.png)  

I will export the data, and convert it to a data frame, to work in R.   

```
StepsByHour <- read_csv("C:/Downloads/stepsbyh.csv")
frameStepsByDay <- as.data.frame(StepsByHour) 
View(frameStepsByDay)  
  
  
ggplot(frameStepsByDay, aes(x=hour, y=StepsByHour),main='Default Axes') + 
geom_bar(stat = "identity") +
scale_y_continuous(name="Thousands of steps", labels = scales::comma)
```  
Now get this graph.  

![image](https://user-images.githubusercontent.com/20979227/181075296-8b0191b5-1a2a-4b0e-98f5-01ed43cd2626.png)  

According to multiple studies, the best hour to go to bed is between 22 and 23 hs.  
In this graph, I see a normal behavior, hours with almost +400.000 steps taken for all the users are vetween 9am to 9pm which is nice.   
But this graph shows a large amount of steps in 23 hs, and 00 hs.  
23 hs is almost 100.000 steps accumulated, and 00:00 are over 50.000. If you divide this numbers, with the information about 33 users you have in this table, you will have:  
  
23:00 hs:  3.000 steps by user  
00:00 hs:  1.500 steps by user   

The numbers at these hours are not particularly recommended.  
Maybe devices can suggest users go to bed when they are reaching 10 pm on their devices.  

## __Share process__  
  
  
 
With the previous query called “Resume general”, I can export it to an excel file and import to Tableau Public and do some visualizations with the information processed.  
Here you can see, Tuesday, Wednesday and Thursday are the day with most calories burned  

![image](https://user-images.githubusercontent.com/20979227/181139423-db85f0cf-fa6a-4ca7-867d-3a50dc22e0ac.png)
   
  
  

Let see the total steps taken by day


![image](https://user-images.githubusercontent.com/20979227/181141053-0ff190b1-a8f7-4f99-b491-56c44c1cc6e1.png)  



It’s ok, it seems to be strongly relationated, how I supposed it would be.

Now let's check about Users.  

![image](https://user-images.githubusercontent.com/20979227/181281699-f54b6342-bbef-49cc-984f-50a15bd3ab4f.png)  


The activities of the users are so different. Some users have a lot of records, some users have a few.  
Maybe some users turn on the devices more times than others.  
It will be helpful to recommend the users turn on the application and device all times, despite the fact that they did not do sports.  
  
  
Finally, I will check the weight table. The users are “fit” users, or not? They’re focused on losing weight or not?  
Check it out.  
In the table weight, I have information about 8 users. It seems to be a few.  
Also, we have just 2 users, with a huge amount of records in the table, so we can not make a confident conclusion about the theory “Are there ‘FIT’ users or not?”  
Anyway, check it out with the information available.  


![image](https://user-images.githubusercontent.com/20979227/181075813-bc2cc656-334b-425e-9667-3f4419b48b75.png)  
  
  

The graph shows the weight in pounds of the users among the exactly dates.    
The biggest difference is in the last user, he/she has 189,2 pounds in 18/4 and also he/she has his/her last record of 185.2 pounds in 12/5.  
There is a difference of 4 pounds, around 1.8 kg in 1 month of 1 user. With just this information, I can see just 1 of 8 users is on the way to weight loss.  
The rest of the users are not looking for weight loss.  



## __Act__  

__Conclusions reached:__  
  
 :mag_right: Users almost reach the double of time to get asleep that normal. It seems to be a problem in the long term.    
 :mag_right: The company should encourage the users to do more sports on weekends, mostly of the people have more free time on weekends, but it is not reflected on the activity they actually do.  
 :mag_right: The company ought to boost people go to bed a couple of hours earlier. Maybe it would be enough with some device notifications, about benefits to go bed at 22 or earlier. Also maybe they get asleep more quickly.  
 :mag_right: Almost ⅓ of the users do not burn the minimum calories a normal human should burn daily. The company must inspire the user try to reach at least 2000 calories daily.  
 :mag_right: Suggest keeping the application and devices turned on, in order to record all the information about their activities, such as a normal day of work, or a lazy day, or maybe a gym day. The company needs to have all the information available posible from the users.  

 
 

  
  
 

















