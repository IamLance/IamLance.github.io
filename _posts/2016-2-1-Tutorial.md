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
**Input Type**:SCM Repository	
**Git URL**:https://github.com//devops-delivery-pipeline.git	
**Branch**:master	
**Stage Trigger**:Run jobs whenever a change is pushed to Git	
3. On the Job Tab, click the ADD JOB link and select Build.Change the job name to any name you want

 **Builder Type**:Gradle	
 **Build Shell Command** 
>  #!/bin/bash
>  gradle assemble


 **Stop running this stage if this job fails**	:checked	
 
4. Click the SAVE button.
Create a Test Stage
-------------
1. Go to the build and deploy tab. Click the ADD STAGE button. Change the stage name to any name you want.
2. On the INPUT tab, set the following values:

	**Input Type**:Build Artifacts	
	**Stage**:Build Stage	
	**Job**:Gradle Assemble	
	**Stage Trigger**:Run jobs when the previous stage is completed	

3.

Create a Deploy Stage
-------------
1. Click the ADD STAGE button. Change the stage name MyStage to any name you want.
2.  On the INPUT tab, set the following values:

	**Input Type**:Build Artifacts	
	**Stage**:Build Stage	
	**Job**:Gradle Assemble	
	**Stage Trigger**:Run jobs when the previous stage is completed	
	
3. On the JOBS tab, click the ADD JOB link and select Deploy. Change the job name Deploy to Cloud Foundry Push to Dev Space. Set the following values:

**Deployer Type**:Cloud Foundry	
**Target**	:IBM Bluemix US South - https://api.ng.bluemix.net	
**Organization**	:leave the default selection	
**Space**	:dev	
**Application Name**	:blank	
**Deploy Script**
	#!/bin/bash
cf push sqlDatabases-<your_name> -m 256M -p build/libs/sqlApp.war	
**Stop running this stage if this job fails**	:checked
4. Click the SAVE button.
Creating SQL Databases Service
-------------
> **Note:**

> - StackEdit is accessible offline after the application has been loaded for the first time.
> - Your local documents are not shared between different browsers or computers.
> - Clearing your browser's data may **delete all your local documents!** Make sure your documents are synchronized with **Google Drive** or **Dropbox** (check out the [<i class="icon-refresh"></i> Synchronization](#synchronization) section).

