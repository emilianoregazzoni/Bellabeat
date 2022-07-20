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
install.package("readr")
install.packages("dplyr")
install.packages("tidyverse")
install.packages("here")
install.packages("janitor")
install.packages("stringr")
library(dplyr)
library(tidyverse)
library(here)
library(skimr)
library(janitor)
library(stringr)
``` 
Import and clean the files: load the .csv files. Remove duplicates and fix inconsistencies (if I can).
