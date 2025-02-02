## Gets started -
Step 1 : Download Jenkins war file - https://jenkins.io/
Step 2 : Place the war file into any location on your system
Step 3 : goto command prompt (windows) | terminal (mac)
   - goto folder where jenkins.war is
   - java -jar jenkins.war
Step 4 : goto browser - http://localhost:8080 (Jenkins window should show up)
Step 5 : install required plugins
Step 6 : get started with Jenkins 

To see hidden files/folders on mac:
1. Exit Finder
2. On terminal - defaults write com.apple.finder AppleShowAllFiles TRUE
3. Press option/alt - right click finder - relaunch

## Running the same previous jenkins with all jobs inside tomcat - ( dont understand and not understand )
enkins Beginner Tutorial 2 - How to setup Jenkins on Tomcat
Why?
Running Jenkins standalone (jetty/winstone) vs Running Jenkins on Tomcat

Pre-requisite
***Tomcat 5 or above***
***java(7 or above) should be available***
(https://www.youtube.com/watch?v=FqpmH...)

Step 1 : Download Tomcat 

Step 2 : Unzip and place tomcat folder at any location

Step 3 : Copy/Place the jenkins.war file inside tomcat/webapps folder

Step 4 : Goto commandpromt (cmd) - windows  |  terminal - mac
  - goto tomcat/bin directory
  - make all files executable : chmod +x *.sh

In case of windows this command will not work. This step might not be needed if you are on windows. Else you can right click on the file/folder goto security tab and change the permissions.

Can watch -   
TOMCAT - How to install and run on Windows | Change Port | Run app
https://youtu.be/ZIIkrn6LHW0?list=PLh...

Step 5 : Start Tomcat : ./startup.sh
  (to shutdown tomcat : ./shutdown.sh)

Step 6 : verify if tomcat started : browser - http://localhost:8080

Step 7 : verify if jenkins is running on tomcat : http://localhost:8080/jenkins

*** To start jenkins (standalone) on a diff port***
java -jar jenkins.war - - httpPort=9090

## To change jenkins home directory in mac windows and linux -

For Windows Users:
If even after changing the Environment variables, the directory does not be change on the  Jenkins webpage
Fix -
 1. Stop Jenkins
 2. Go to Jenkins installed folder c:\Program Files\Jenkins (or where ever the Jenkins folder was created at the time of installation) 
 3. Modify the jenkins.xml file with ＜env name="JENKINS_HOME" value="C:\Program Files\Jenkins\JenkinsHome"/＞
and comment the old ＜env name＞ 

jenkins Beginner Tutorial 3 - How to change Home Directory
Jenkins Home Directory contains:
1. All configurations
2. Plugins
3. Jobs details
4. Logs

Why:
-To move jenkins home dir to a location that has enough disk space
-Project requirements

If you have installed jenkins on windows through msi file then read this - http://tech.nitoyon.com/en/blog/2014/...

Step 1 : Check your current home directory

Step 2 : Create a new folder (which will be new home dir)

Step 3 : Copy all data from old dir to new dir

Step 4 : change env variable - JENKINS_HOME and set to new dir
    Windows - change env variable
   Mac - goto terminaal
 export JENKINS_HOME=/Users/raghav/Desktop/Tools/Jenkins/JenkinsHome/
 mac - open .bashprofile and paste export JENKINS_HOME=/Users/raghav/Desktop/Tools/Jenkins/JenkinsHome/

Step 5 : restart Jenkins

____________________________________________________________

To see hidden files/folders on mac:
On the Finder window press Cmd+Shift+.
OR
1. Exit Finder
2. On terminal - defaults write com.apple.finder AppleShowAllFiles TRUE
3. Press option/alt - right click finder - relaunch

====================================================== .
## How to use Jenkins from command line
Step 1 : start Jenkins

Step 2 : goto Manage Jenkins - Configure Global Security - enable security && authorization -anyone can do anything

Step 3 : goto - http://localhost:8080/cli/

Step 4 : download jerkins-cli jar. Place at any location.

Step 5 : test the jenkins command line is working
 Step 6:  if it needs any passowrd then go to jenkins click on dropdown of of username, select configure, pass ssh public key like - 1234
 
## How to create Users + Manage + Assign Roles
Jenkins authentication and authorization
Today we will learn:
———————————
How to create New Users
How to configure users
How to create new roles
How to assign users to roles
How to Control user access on projects


Step 1 : Create new users

Step 2 : Configure users

Step 3 : Create and manage user roles Roles Strategy Plugin - download - restart jenkins

Step 4 : Manage Jenkins - Configure Global Security - Authorisation - Role Based Strategy

Step 5 : Create Roles and Assign roles to users

Step 6 : Validate authorization and authentication are working properly

## Basic Configurations
========================================

Step 1 : Goto Manage Jenkins - Configure System

Step 2 : Get a basic understanding of common/basic configurations

