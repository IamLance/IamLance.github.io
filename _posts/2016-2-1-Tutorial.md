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


Examine the code - Finding the Service
-------------
 The Application is coded in JSP.
 
1.  Examine the Database connection code. Go to DBHelper.java

Notice that with this line we get the system environment variables

    Map<String,String> env = System.getenv();

 Then we check if it contains a VCAP services

    if(env.containsKey("VCAP_SERVICES"))

If it does contain a VCAP services we then  put it in a JSON object since it is in JSON format.

      JSONObject vcap = (JSONObject)       parser.parse(env.get("VCAP_SERVICES"));
  

We then parse it and look for the SQL Database service hiding under the key ""sqldb"".

      for (Object key : vcap.keySet()) {
                String keyStr = (String) key;
                if (keyStr.toLowerCase().contains("sqldb")) {
                    service = (JSONObject) ((JSONArray) vcap.get(keyStr)).get(0);
                    break;
                }
            }
We then check if it was able to find the SQL Database service 

     if (service != null) 

and If it was able to find the service we then get the credentials we need to connect to the database

    JSONObject creds = (JSONObject) service.get("credentials");
                databaseHost = (String) creds.get("host");
                databaseName = (String) creds.get("db");
                port = (long) creds.get("port");
                user = (String) creds.get("username");
                password = (String) creds.get("password");
                url = (String) creds.get("jdbcurl");
                
            

Examine the code - Connecting to the Database
-------------

    DB2SimpleDataSource dataSource = new DB2SimpleDataSource();
                    dataSource.setServerName(databaseHost);
                    dataSource.setPortNumber((int) port);
                    dataSource.setDatabaseName(databaseName);
                    dataSource.setUser(user);
                    dataSource.setPassword(password);
                    dataSource.setDriverType(4);