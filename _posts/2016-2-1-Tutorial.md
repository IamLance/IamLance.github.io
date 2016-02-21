---
layout: post
title: 
---

SQL Database  
===================


SQL Database stores data that your app manages. Specifically uses for  transactional purposes. 

In Bluemix it is powered by DB2.

----------


Installing the Application
-------------
In this part we will be installing your the application by uploading it to bluemix.

 1. Clone the respository https://github.com/IamLance/sql-databases
> git clone https://github.com/IamLance/sql-databases


 2. Go to the directory where you cloned the repository 
 3.  In the command prompt login to you bluemix account

> cf login -a https://api.ng.bluemix.net -s dev


 Then Push the App

    > cf push sqlTutorial-(your_name) -m 256M -p sql-databases/build/libs/sqlApp.war

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


Code Analysis - Finding the Service
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
                
            

Code Analysis - Connecting to the Database
------------- 
To connect to the database we then input the credentials we parse earlier from the VCAP services and input in DB2SimpleDataSource to get a connection. 

    DB2SimpleDataSource dataSource = new DB2SimpleDataSource();
                    dataSource.setServerName(databaseHost);
                    dataSource.setPortNumber((int) port);
                    dataSource.setDatabaseName(databaseName);
                    dataSource.setUser(user);
                    dataSource.setPassword(password);
                    dataSource.setDriverType(4);
                    this.con = dataSource.getConnection();
                

Code Analysis - SQL Statements
------------- 
Code for creating a table 

        private void createTable() {
        // create a table
        if (con != null) {
            try {
                stmt = con.createStatement();
                // Create the CREATE TABLE SQL statement and execute it
                sqlStatement = "CREATE TABLE IF NOT EXISTS " + tableName
                        + "(FNAME VARCHAR(20), LNAME VARCHAR(20))";
                writer.println("Executing: " + sqlStatement);
                stmt.executeUpdate(sqlStatement);
            } catch (SQLException e) {
                writer.println("Error creating table: " + e);
            }
        }

    }

Code for inserting data

     public String insertInto(Account bean) {
            if (con != null) {
                try {
                    sqlStatement = "INSERT INTO " + tableName
                            + "(FNAME,LNAME) VALUES (?,?)";
                    PreparedStatement preparedStatement = con.prepareStatement(sqlStatement);
                    preparedStatement.setString(1, bean.getFname());
                    preparedStatement.setString(2, bean.getLname());
                    writer.println("Executing: " + sqlStatement);
                    preparedStatement.executeUpdate();
    
                } catch (SQLException e) {
                    writer.println("Error " + e);
                }
                return selectSingle(bean.getFname());
            }
            return null;
        }
    

Code for Selecting data

    public ArrayList<Account> getAllAccount() {
            if (con != null) {
                try {
                    sqlStatement = "SELECT * FROM " + tableName;
                    ArrayList<Account> beans = new ArrayList<>();
                    ResultSet rs = stmt.executeQuery(sqlStatement);
                    writer.println("Executing: " + sqlStatement);
                    while (rs.next()) {
                        Account a = new Account();
                        a.setFname(rs.getString("fname"));
                        a.setLname(rs.getString("lname"));
                        beans.add(a);
                    }
                    return beans;
                } catch (SQLException e) {
                    writer.println("Error " + e);
                    return null;
                }
            }
            return null;
        }

Code for deleting data

     public void deleteAllAccount() {
            if (con != null) {
                try {
                    sqlStatement = "delete FROM " + tableName;
                    ArrayList<Account> beans = new ArrayList<>();
                    stmt.executeUpdate(sqlStatement);
                } catch (Exception e) {
                }
            }
        }




dashDB  VS SQL Database
===================
1. Login to your Bluemix account [Link](https://console.ng.bluemix.net/)
2.  Go to catalog and find dashdb and click it then  click create leave the service unbound.
3.  On the right side click the launch button.
4.  It will open another tab we will call this the dashdb console. Go back to the previous tab and go to dashboard. 
5.  In the dashboard find the previous SQL Database service we have created in the SQL Database Tutotrial and click it.
6.  On the right side click launch. It will launch another tab and we will call this the SQL Database console.
7.  Exploring both you will notice that dashdb has more features than SQL Database.
8.  In the dashdb console click analytic on the left side then click R script.
9.  You will see the sample projects In-Application Analytics, In-Database Analytics and Data-Visualization all these not existing in SQL Databases.
10.  Click the Customer-Churn(Decision Tree) under the In-Database analytics.
11. You will see the sample script in the script tab. This is actually a R script that is mostly common use in data analysis and statistics.
12.  Click submit.
13.  In the Plot tab you will see the result of the R script code.
14.  Explore other features of dashdb by repeating steps from 10 to 13 except this time picking a different option.

----------

dashDB  
===================
Is a data warehousing and also a data analytics tool for analysing data and creating deep insights.