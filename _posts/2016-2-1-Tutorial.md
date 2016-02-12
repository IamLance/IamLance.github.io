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
In this part we will be installing your the application using the [Bluemix DevOps Services](https://hub.jazz.net/)  It has a delivery pipeline that allows you to build, test, and deploy your web application.

 1.  Open a web browser tab and login to [GitHub](https://github.com).
 2. Go to the GitHub repository and fork https://github.com/IamLance/sql-databases.
 3. Open another web browser tab and login to [Bluemix DevOps](https://hub.jazz.net/).
 4. Click CREATE PROJECT.
 5.  Name your project sql-databases
 6. Click Link to an existing GitHub repository.
 7. Select the repository <username>/sql-databases
 8. Ensure the following options are chosen:

	 **Private Project**:checked	

	**Make this a Bluemix Project**:checked	

	**Region**:IBM Bluemix US South	

	**Organization**:leave the default selection	

	**Space**:dev   
  
 9. Click the CREATE button. Wait for your project to be created.

 Create a Build Stage
-------------
1. Go to the build and deploy tab. Click the ADD STAGE button. Change the stage name to any name you want.
2.  On the INPUT tab, set the following values:
**Input Type**:SCM Repository	
**Git URL**:https://github.com//devops-delivery-pipeline.git	
**Branch**:master	
**Stage Trigger**:Run jobs whenever a change is pushed to Git	
3. On the Job Tab, click the ADD JOB link and select Build.Change the job name to any name you want

 **Builder Type**:Gradle	

>  **Build Shell Command** 
>  #!/bin/bash
>  gradle assemble


 **Stop running this stage if this job fails**	:checked	

> **Note:**

> - StackEdit is accessible offline after the application has been loaded for the first time.
> - Your local documents are not shared between different browsers or computers.
> - Clearing your browser's data may **delete all your local documents!** Make sure your documents are synchronized with **Google Drive** or **Dropbox** (check out the [<i class="icon-refresh"></i> Synchronization](#synchronization) section).

