# Layer7Monitor
Complete monitoring solution for CA Layer 7 API Gateway.

Table of Content

1. Overview

        1.1. Introduction
        
        1.2. Advantage
        
        1.3. Scope of monitoring
        
        1.4. Quick Installation
        
2. Tool technical overview

        2.1. Tool Directory layout
        
        2.2. Execution Process
        
        2.3. Configuration and History Management overview
        
        2.4. Toubleshooting steps
        
        2.5. Incident Management Tool Integration Process
        
3. Property file overview and details of property monitored

4. Support and managebility

## 1. Overview

### 1.1. Introduction

This tool is build to monitor a complete CA Layer 7 API Gateway Farm. The tool is meant to be a complete monitoring solution to monitor all aspects of Gateway Infrastructure and using single instance of this tool we will be able to monitor all Gateway Clusters in your enterprise.

### 1.2. Advantage

The tool does different type of validation for different aspects/components of a Gateway Infrastructure. You should setup the property file to define your whole Gateway Infrastructure. Together its builds a complete end to end monitoring solution:

        a. Monitor the health check API build for all the Gateways in the enterprise.
        b. Monitor CPU/Memory/Disk for all Gateways hosts in the enterprise.
        c. Monitor database conectivity status for all DB used in all Gateways in the enterprise.
        d. Monitor internal MySQL DB replication status for all MySQL Clusters used in all Gateways in the enterprise.
        e. Monitors Certificate for expiry installed in the Certificate Stores in all Gateways in the enterprise.

The tools uses a completely modular approch for different components to be monitored. Thus it is completely flexible in adding and removing monitoring modules to the tool.
Also one of its design goal is to be fast and thus eficient, in acheiving this goal the tool uses parallel module execution. This reduce module dependency and fast execution cycle.
In addition the tool has an self issue detection mechanism thus any suspected issue in a perticular monitoring cycle can/should be detected by the tool itself and should be reported to the admins.
The tool is build not to send repeated alerts thus reducing the noise during an ongoing issue.

### 1.3. Scope of monitoring

At this point of time the tool can be used to monitor any API Gateway Installation. The tool relies heavily on SSH based connection to collect host statistics thus password less login is required at this point for host monitoring.

Since tool is Unix Bash based it is only limited to Unix environment for its execution.

### 1.4. Quick Installation

This probably is the best part of the tool. The installation require minimum effort, but before you install please make sure the below is available in your environment.

        a. Utility that should be present are quite basic core Linux utility mostly comes with Default Linux build. Eg: curl, ssh.
        b. SMTP Setup is required for the tool to send Alerts for issue detection.
        
**Custom API Creation**

For few of the monitoring scenario one need to publish some custom API which is used by the script for monitoring.

Hello API

DB Connectivity Check API

Certificate Expiry Check API


The installation require the tools directory to be placed on any location on the host where the tool need to be executed. This is all it takes to install the tool. But please do go though the Tool technical overview section to understand how the tool should be setup for monitoring and finally initiate the actual monitiong. 

## 2. Tool technical overview



### 2.1. Tool Directory layout


[DIRECTORY] **tmp** - Temporary work area. Files inside it can be deleted after execution but not during execution.

[DIRECTORY] **log** - Log directory. 

[DIRECTORY] **history** - Contains report file for Incident Management integration and detecting previous alert published for stopping repated alerts. 

[DIRECTORY] **mail** - Directory containing mail templetes. 

[FILE] **layer7monitor.prop** - Main property file. This need to be filled for the tool to be operational.

[FILE] **urlmonitor** - Module to monitor health check URL. 

[FILE] **hostmonitor** - Module to monitor health check of host. 

[FILE] **dbmonitor** - Module to monitor health check Database. 

[FILE] **certmonitor** - Module to monitor expiry Certificate installed. 

[FILE] **mailsend** - Module to email alerts.

[FILE] **layer7monitor** - Main executor script. This is to be executed from initiating the monitoring.

### 2.2. Execution Process

The tool can be executed by

[SCRIPT HOME]/layer7monitor 

***NOTE:*** Its imporatant to understand that just executing the monitor will not allow you to continuously monitor the environment and we should setup some kind of repeated execution mechanism via your Enterprise Scheduler, e.g. ControlM. As a sample setup the below example will help you execute the monitor in a periodically basis in a ***once every 1 hours*** using Unix default scheduler Crontab.

> `05 * * * * [SCRIPT HOME]/layer7monitor >> [SCRIPT HOME]/log/monitor.cron.log 2>> [SCRIPT HOME]/log/monitor.cron.log

### 2.4. Toubleshooting steps

The script has been build to log fairly verbose log level once you enable debug on, so that any issue if occured can be identified using the log only. The log for the all modules along with the monther monitoring process goes in one single log stated below:

LOCATION | LOG NAME
--- | ---
log | **layer7monitor.log**

***NOTE*** 

        a. The tool keeps the log file to a specified size ([PROPERTY] layer7.logsizemb) in MB and autorotate keeping one additional backup file.
        b. In case debug logs required for toubleshooting issue in script, it can be enabled by setting property ([PROPERTY] layer7.debug) to yes.
        c. To identify issue specific to a module filter logs as following. Ex: URL -> ": URL :", Host -> ": HOST :", DB -> ": DB :", Cert -> ": CERT :".


### 2.5. Incident Management Tool Integration Process

The tool can be easily intergrated with your current enterprise monitoring system to generate Incidents in your standard Enterprise Incident Management system. The below mechanism should be followed for the integration.

* Enable Log Watcher module in your enterprise monitoring system on the monitoring host.
* Configure Log Watcher to search for pattern ***:ERROR:*** in report file inside history directory.


## 3. Property file overview and details of property monitored

The propery file is the key to configure monitoring for your environment. 


## 4. Support and managebility

If you are reading this README file then you are probably about to use the my tools to help you monitor your API Gateway Infrastructure. Good choice. This tool is made for you. Moreover this tool is free and always will be thats my promise.

Now it is hard to believe that you will get 24/7 Support thats too much to ask for. But in case you face any issue and want my intervention and you cannot debug the hundreeds lines of core Bash Script your self, please do not hassitate to write to me. Its a guarentee you will get an answer but it is not a guarentee you will have it in a SLA.

Reach Me: sanmuk21@gmail.com

Best of luck. Happy Monitoring your API Gateway Infrastructure.
