# YouTube Data Harvesting and Warehousing:
  > Using Python, MYSQL, MongoDB, Streamlit and Googleapiclient

## Project Descriptions:

- The problem statement is to create a Streamlit application that allows users to access and analyze data from __Multiple YouTube Channels__:
   
   - Ability to input a _**YouTube channel ID**_ and retrieve all the relevant data using _**Google API**_.
  
        __| Channel name | Subscribers | Total video count | Playlist ID | Video ID | Likes| Comments of each video |__
     
   - Option to store the data in a MongoDB database as a data lake.
   - Ability to collect data for up to 10 different YouTube channels and store them in the data lake by clicking a button.
   - Option to select a channel name and migrate its data from the data lake to a SQL database as tables.
   - Ability to search and retrieve data from the SQL database using different search options, including joining tables to get channel details.




 ## Basic Requirements:

- __[Python 3.11](https://www.google.com/search?q=docs.python.org)__
- __[googleapiclient](https://www.google.com/search?q=googleapiclient+python)__ 
- __[mysql_connector](https://www.google.com/search?q=mysql+connector)__ 
- __[Pandas](https://www.google.com/search?q=python+pandas)__
- __[Streamlit](https://www.google.com/search?q=python+streamlit)__
- __[Numpy](https://www.google.com/search?q=numpy)__ 
- __[pymongo](https://www.google.com/search?q=pymongo)__
- __[requests](https://www.google.com/search?q=requests)__


<p align="center">
  <img src="https://github.com/pnraj/Projects/assets/29162796/72ee83a0-501d-4fae-b474-bd42fb49e101" alt="Project WorkFlow">
 </p>

## General BackEnd WorkFlow Of This Project:
1.__Api Call And Data Sorting:__

  - _Based On The Users Need, Users Can Fetch Data From YouTube By Entering Url or Keyword_ 
  - To Make This Work I Designed Two Separate Files To Make Api Calls **_Single_Channel.py_** and **_Multi_Channel.py_** using **_Googleapiclient_**
      > which is inside off __Yapi__ Directory Of This Repo
  - After Data Got Fetched it is Shown as Three Separate Section Chanenl Details, Video Details, Comments Details
  - For Sorting And Isolation of Values From Data I have Used **_Pandas_** 
  - For Visualize The Data I Had Used **_Streamlit_** Inbuilt markdown features along with html 
  
2.__Uploading To MongoDb Atlas:__
    
  - Api call Gets Data in _JSON_ Format with lots of Details in each catagories:[Youtube Docs](https://developers.google.com/youtube/v3/docs/)
  
  ``` py
                    1. Channels
                    2. Playlist
                    3. Videos
                    4. Comments
                    5. Search and many more
  ```
  
  - Data get Formated and Made Ready for Users to Upload to MongoDB which is **_Data Lake_** 
  - In MongoDB Each users Data is Stored in DB Called `youtube` and Collections name is Created based upon on the users Channel search
  - Sample of Data are shown to Users in **_Streamlit_** App After Succesfull Insert of Data into _[MongoDB Atlas](https://mongodb.com/)_

##### MongoDB Data Sample
  ``` json

          {
  "_id": "UCLIpo6xntiT-H0P4wLejpcg",
  "Channel_Name": "Viswa Agz",
  "Channel_data": {
    "Channel_Details": {
      "Channel_Name": "Viswa Agz",
      "Channel_Id": "UCLIpo6xntiT-H0P4wLejpcg",
      "Video_Count": "30",
      "Subscriber_Count": "249",
      "Channel_Views": "36880",
      "Channel_Description": "எல்லோரும் பயணிக்கிறார்கள் என்று நீயும் பின்தொடராதே உனக்கான பாதையை நீ...",
      "Playlist_Id": "UULIpo6xntiT-H0P4wLejpcg"
    }
  },
  "Video_Id_1": {
    "Video_Id": "7WgWtTcvrNw",
    "Video_Name": "SNS ANNUAL DAY VLOG | #gvprakash Stage performance 🎤|",
    "Video_Description": "**************************************************************\n\nDISCLA…",
    "Tags": [],
    "PublishedAt": "2023-04-09T02:21:03Z",
    "View_Count": "259",
    "Like_Count": "22",
    "Dislike_Count": 0,
    "Favorite_Count": "0",
    "Comment_Count": "5",
    "Duration": "00:20:32",
    "Thumbnail": "https://i.ytimg.com/vi/7WgWtTcvrNw/hqdefault.jpg",
    "Caption_Status": "false",
    "Comments": {
      "Comment_Id_1": {
        "Comment_Id": "UgxVjcxtVUIkl-Bojrp4AaABAg",
        "Comment_Text": "Video quality 720p la paru ga ❤",
        "Comment_Author": "Viswa Agz",
        "Comment_PublishedAt": "2023-04-09T02:53:56Z"
      }
    }
  }
}
  ```




3.__Uploading To Mysql DataBase:__

   - Data From MongoDB are then Converted into Tables and Rows using __Pandas__ with Normalization of Values are ready to Upload to MysqlDB
   - **_Mysql Connector_** is used for Connecting App and MysqlDB 
   - In Multiple Channel Mode Users Have Options of Choosing Channels That Needs to Be Uploaded to MysqlDB
   - MysqlDB will have Three Tables **_Channel_Table, Videos_Table, Comments_Table_** 
   
   ``` sql
           
        CREATE TABLE IF NOT EXISTS channel (
                  Channel_Name            VARCHAR(255),
                  Channel_Id              VARCHAR(255),
                  Video_Count             INT,
                  Subscriber_Count        BIGINT,
                  Channel_Views           BIGINT,
                  Channel_Description     TEXT,
                  Playlist_Id             VARCHAR(255))
          
        CREATE TABLE IF NOT EXISTS playlist (
                  Channel_Id     VARCHAR(255),
                  Playlist_Id    VARCHAR(255))
          
        CREATE TABLE IF NOT EXISTS video (
                Playlist_Id         VARCHAR(255),
                Video_Id            VARCHAR(255),
                Video_Name          VARCHAR(255),
                Video_Description   TEXT,
                Published_Date      VARCHAR(50),
                View_Count          BIGINT,
                Like_Count          BIGINT,
                Dislike_Count       INT,
                Favorite_Count      INT,
                Comment_Count       INT,
                Duration            VARCHAR(1024),
                Thumbnail           VARCHAR(255),
                Caption_Status      VARCHAR(255))
    
        CREATE TABLE IF NOT EXISTS comments (
                  Video_Id              VARCHAR(225),
                  Comment_Id            VARCHAR(225),
                  Comment_Text          TEXT,
                  Comment_Author        VARCHAR(225),
                  Comment_Published_date VARCHAR(50)
   ```
   
   
   
   
![Youtube Data Modelling](https://github.com/Knivash/Youtube-Harvesting-Data/assets/136495926/92300366-e449-43b6-bd67-c67b2e69878e)


  

4.__Querying From Mysql DataBase:__

   - Querying Data From MysqlDB Have Two Options With **_Pre-Defined Query_** and **_Custom Query_**
   - **_Pre-Defined Query_** will display details of the Selected Channels or Channels that are currentely Shown in Channels List 
   - **_Custom Query_** will have options to use **_SQL_** Queries to get Details of all Channels, Videos, Comments Using all basic _SQL_ Commends like **_JOIN,WHERE,SELECT,GROUP BY, HAVING_**  

``` SQL
        SELECT channel.Channel_Name, video.Video_Name, video.Comment_Count
        FROM channel JOIN playlist
        ON channel.Channel_Id = playlist.Channel_Id
        JOIN video ON playlist.Playlist_Id = video.Playlist_Id
        ORDER BY video.Comment_Count DESC;

```



