# 2018-Bay-Riders

Introduction

Before the start of the pandemic, if you were to walk around any city in the world you would come across a bike share system.  City’s will offer a bike share as an affordable, easy way for people to ride around their city. A large city like San Francisco introduced a bike share system back in 2017 with the bikes being presented by Lyft, the common ride share company that millions of people use each and every day. Lyft has developed a way on their app for users to find bikes across the city for their users and have documented their data for public use. Being that I am from New York I have seen a variation of the bike share used in the city and people from all over use the bikes to commute to work or travel around to see the sites. The reason that I chose this data was that I was curious to see how long people rode the bike for and how far people rode these bikes. My initial thought was that users would rent these bikes just to commute to and from work but what I found was quite interesting. 

Data
On Lyft’s website that have documented each bike ride monthly and it is free to download for research purposes. Each month is separated in its own csv file so that it can be easily accessed. The data included is anonymous and includes the following data points:
•	Trip Duration in seconds
•	Start Time and Date
•	End Time and Date
•	Start Station ID
•	Start Station Name
•	Start Station Latitude
•	Start Station Longitude
•	End Station ID
•	End Station Name
•	End Station Latitude
•	End Station Longitude
•	Bike ID
•	User Type (Subscriber or Customer)

At first glance I didn’t think that Lyft was giving up that much information but with some manipulation I was able to see a little deeper. Initially I was interested at the difference between subscribers and customers but I realized I would be able to dig a little deeper. Since we were given duration in seconds I thought I would be able to expand that into minutes and hours to make the duration more visually appealing. Lyft also gave the start latitude and longitude along with the end latitude and longitude which I thought that I would be able to find the distance traveled between stations.  

# Assessing & Cleaning Data

As I mentioned, Lyft has saved each bike ride per month in separate files starting in 2017. For this analysis I downloaded each monthly file for the year of 2018 to look into my initial thoughts. Once downloaded I saved the files into one folder so that they would be easily accessible. From there I created a loop to import the data in to Python and place it into a Pandas dataframe since cleaning the data would be easily done in a dataframe.
Once imported I looked at the info of the data and the data head to have an idea of what I was looking at. The data info gave me a lot of information, which is below for a reference:

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1863721 entries, 0 to 1863720
Data columns (total 14 columns):
    Column                   Non-Null Count    Dtype  
---  ------                   --------------    -----  
 0   duration_sec             1863721 non-null  int64  
 1   start_time               1863721 non-null  object 
 2   end_time                 1863721 non-null  object 
 3   start_station_id         1851950 non-null  float64
 4   start_station_name       1851950 non-null  object 
 5   start_station_latitude   1863721 non-null  float64
 6   start_station_longitude  1863721 non-null  float64
 7   end_station_id           1851950 non-null  float64
 8   end_station_name         1851950 non-null  object 
 9   end_station_latitude     1863721 non-null  float64
 10  end_station_longitude    1863721 non-null  float64
 11  bike_id                  1863721 non-null  int64  
 12  user_type                1863721 non-null  object 
 13  bike_share_for_all_trip  1863721 non-null  object 
dtypes: float64(6), int64(2), object(6)
memory usage: 199.1+ MB

The info function told us that there were 1,863,721 records or bike rides during 2018 in San Francisco. I also noticed that the start and end times were objects rather than date and time format. I also saw that there was missing information for the start and end stations but I think the reason for this is that the bike was not picked up from a station and could have been picked up from a street corner.

From there I ran a describe function on the data to see if I would be able to find any other insight but it did not give me much to go by. I also checked if there were ant duplicates within the data since there was not a unique identifier for each ride. Once I ran the duplicated function, I was delighted to see that there were no duplicates in the data. But given there were no duplicates with over a million rides I was interested, to see how many bike id’s were duplicated. When looking at the value counts there are various bike ids with counts over 1000 but I can conclude that while some bikes have over 1000 rides that does not mean they were duplicated. 

The main thought I had was how many riders were customers and how many were subscribers. To my surprise, subscribers out weighted the customers 1.5 million to 280,000 thousand. This means that there are far less one time customers than daily users. 
Once I was done assessing my data I put a list together of what I needed to do to clean my data. My cleaning steps are as follows:
1.	Start and end time should be changed to date and time format
2.	User_type should be changed to category
3.	Bike share should be changed to category
4.	Change duration from seconds to hours
5.	Separate the start date to month column
6.	Add a column for miles traveled by using start longitude/latitude and end longitude/latitude 
