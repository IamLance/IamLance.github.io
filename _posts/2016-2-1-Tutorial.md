---
layout: post
title: 
---

SQL Database and dash DB Tutorial
===================


SQL Database stores data that your app manages. Specifically uses for  transactional purposes. 

In Bluemix it is powered by DB2.

----------


Installing the Application
-------------
In this part we will be installing your the application by uploading it to bluemix.

 1.  Open a web browser tab and login to [GitHub](https://github.com).
 2. Download [AppHere](https://github.com/IamLance/sql-databases/blob/master/build/libs/sqlApp.war)
 3. In your Desktop create a new Folder and name it sqlTemp 
 4.  Copy the downloaded App to sqlTemp
 5.  Using the command prompt go to the sqlTemp directory
 6.  In the command prompt login to you bluemix account

> cf login -a https://api.ng.bluemix.net -s dev


 Then Push the App
> cf push sqlTutorial-(your_name) -m 256M -p sqlApp.war

Add a SQL Database Service
-------------
In this part we will be create a SQL Database Service and bind it our app


1. Login to your Bluemix account [Link](https://console.ng.bluemix.net/)
2.  Go to you dashboard.
3.  Under the Application look for the sqlTutorial-(your_name) and click it.
4.  Click add a Service or API and you will be redirected to the Catalog
5.  Under the catalog find the SQL Database and click it.
6.  Click create.
7. Restage Application


Examine the code
-------------
 The Application is coded in JSP.
 
1.  Examine the Database connection code. Go to DBHelper.java

Notice that with this line we get the system environment variables

Map< String, String > env = System.getenv();
 