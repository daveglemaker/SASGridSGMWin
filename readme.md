# SAS Grid Manager CLI and SASGSUB Macros for Windows
           SAS version: SAS 9.4M7 SAS Grid Manager 
                        SAS® Workload Orchestrator Administration Utility,
                        Version 9.4M7 (build date: Aug  5 2020 @ 20:18:50)

Usage and Setup can be found in "SAS Grid and Gsub Macros 2019 SGM" powerpoint file


## Macros for the SAS User Role
- %mygsub %mygsubque - submit a .sas program with sasgsub to the default queue or a named queue
- %gjobs and %gsstatus – reports on job status
- %gsresults – move job results to a different directory
- %gskill – terminate a gsub job

## Macros for the SAS Admin role
- %gjobs – reports on job status of all users
- %bhosts – reports on Grid node status and number of jobs by machine
- %bqueues - reports on queue priority, status, and number of jobs by queue
- %lsload – reports on Grid node resources
- %kill – terminates a grid job
- %resume – resumes a suspended grid job
- %suspend – suspends a grid job

## Install Steps
1. Copy all .sas Files to ...SASCOMPUTECONFIGDIR.../Lev1/SASApp/SASEnvironment/SASMacro  
           repeat for each server context as desired, i.e. Server context = SASApp
    
2. Edit ...SASCOMPUTECONFIGDIR.../Lev1/SASApp/appserver_autoexec_usermods.sas with code below for mygsub and CLI macros to work  
           change "Yourconfigdir" to your actual configuration directory and "sasgridswomasterhost" to your master host name.
           if your home directory (~) isn't a shared location that all nodes can access then change to a shared location unique to each user, both options are below

           %let gsconfigdir=Yourconfigdir/Lev1/Applications/SASGridManagerClientUtility/9.4;   
           %let gauconfigdir=/Yourconfigdir/Lev1/Applications/GridAdminUtility/;   
           %let mhost=sasgridswomasterhostname;   
           %let mport=8901;  
           %let authinfo=~_authinfo; * or ; %let authinfo=S:\myshared_dir\%sysget(USERNAME)\_authinfo;
           
- Be sure to update your sasgsub.cfg to not prompt for password
- CLI macros assumes _authinfo file exist in shared directory in folder of username 
-- Sample content of _authinfo file: default user sasdemo password {SAS002}1D57933958C580064BD3DCA81A33DFB2
-- They can do a onetime run of the below code (substituting in their password and the shared drive that matches what you put in the autoexec_usermods for %let authinfo=). Reruns needed if their password changes.
---options noxwait;
---data _null_;
---   x 'mkdir S:\myshared_dir\%USERNAME%';
---   x 'echo default user %USERNAME% password {SAS002}1D57933958C580064BD3DCA81A33DFB2 >> S:\myshared_dir\%USERNAME%\_authinfo"';
---run;

           
### SMC setup
#### Enable xcmd, repeat for each server context desired, i.e. Server context = SASApp
 In SAS Management Console enable xcmd on Logical Workspace Server:
 1. Expand 'Server Manager'
 2. Expand Server Context i.e. SASApp
 3. Expand Logical Workspace Server i.e. 'SASApp - Logical Workspace Server'
 4. Right Click on Workspace Serve 'SASApp - Workspace Server' and Select Properties
 5. Click on 'Options' Tab
 6. Click on 'Advanced Options' Button
 7. Click on 'Launch Properties' Tab
 8. Click on 'Launch Properties' Tab
 9. Check the 'Allow XCMD' Box
 10. Click 'OK'
 11. Click 'OK'
#### Refresh Spawner
 In SAS Management Console Refresh Spawner:
 1. Expand 'Server Manager'
 2. Expand 'Object Spawner - YourServername
 3. Right Click on each Server and select 'Connect'
 4. Right Click on each Server and select 'Refresh Spawner' and click 'Yes',  Note: clicking 'Refresh' will not work, you must select 'Refresh Spawner'
    
 Now all NEW SASApp Sessions will be able to run SAS Grid Macros

## Disclaimer
These macros are made available “as is” and disclaims any and all representations
and warranties, including without limitation, implied warranties of
merchantability, accuracy, and fitness for a particular purpose.
