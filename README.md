[![Build Status](https://travis-ci.org/jhigman/sprat.png)](https://travis-ci.org/jhigman/sprat)


Sprat : Spreadsheet API Test Runner
===================================

Write tests for an API in a spreadsheet. Then run the spreadsheet.
------------------------------------------------------------------  

The spreadsheet is in Google Drive, and looks like this:


![Sprat MD5 Test](images/sprat-md5.png)  



The tests are run from the Test Runner menu, and the spreadsheet updated with the results. 

Try the [demo spreadsheet](https://docs.google.com/spreadsheet/ccc?key=0AnNso1xhxP7xdEpmb3prMWdmMEF6Ti05c29TT3R4Q0E#gid=0) (the script will prompt for permissions when invoking the Test Runner for the first time)

Tests can also be queued, and results viewed, from the [Test Runner UI](http://serene-shore-1334.herokuapp.com/jobs).


Running the app
---------------


To run the web app:

	bundle exec rackup config.ru

See the homepage:

	http://localhost:9292


Accessing Google Spreadsheets
-----------------------------

Set some environment variables to give access to Google Spreadsheets:

	GOOGLE_DRIVE_USERNAME=username for google drive spreadsheets account
	GOOGLE_DRIVE_PASSWORD=password for google drive spreadsheets account


Running background jobs
-----------------------

Run a Resque worker process to process background jobs:

	QUEUE=test_jobs bundle exec rake resque:work


Using RedisCloud
----------------

By default, the app will use the local Redis client.

To use a RedisCloud instance, set this environment variable:

	REDISCLOUD_URL=url for redis cloud instance, e.g. redis://rediscloud:1234567890@pub-redis-15001.us-east-1-1.2.ec2.garantiadata.com:15001



Script to run tests from a Google Drive Spreadsheet
---------------------------------------------------

There's a library of Google App Script helper functions published by the Sprat Google account. 

The Project Key for the published library is : *MNMuL6AHdxrq9UX44qx2DMKxz0Q3oDASm*

To add the library to your spreadsheet, use Tools->Script Editor to open the script editor, then Resources->Libraries to show the libraries dialog. Then search for the Sprat Library by Project Key (as above) and add the latest version to your scripts. (You may also need to enable the "Development Mode" flag).

Then, add this code to a new script, after which a "Test Runner" menu option will appear when you re-open the spreadsheet:


    function onOpen() {
      var ss = SpreadsheetApp.getActiveSpreadsheet();
      var menuEntries = [ {name: "Run tests", functionName: "runThisSheet"} , {name: "Schedule tests", functionName: "scheduleThisSheet"} ];    
      ss.addMenu("Test Runner", menuEntries);    
      setTestRunnerProperties();    
    }    
    
    function setTestRunnerProperties() {
      ScriptProperties.setProperty("TestRunnerURL", "http://serene-shore-1334.herokuapp.com/jobs");    
      ScriptProperties.setProperty("TestRunnerAuth", "");    
    }    
    
    function runThisSheet() {
      testRunnerURL = ScriptProperties.getProperty("TestRunnerURL");
      testRunnerAuth = ScriptProperties.getProperty("TestRunnerAuth");
      runTestJob(testRunnerURL, testRunnerAuth);
    }
    
    function scheduleThisSheet() {
      testRunnerURL = ScriptProperties.getProperty("TestRunnerURL");
      testRunnerAuth = ScriptProperties.getProperty("TestRunnerAuth");
      scheduleTestJob(testRunnerURL, testRunnerAuth);
    }



Authors
-------

**Julian Higman**

+ [http://twitter.com/jhigman](http://twitter.com/jhigman)
+ [http://github.com/jhigman](http://github.com/jhigman)

**Matt Law**

+ [http://twitter.com/staringskyward](http://twitter.com/staringskyward)
+ [http://github.com/staringskyward](http://github.com/staringskyward)
