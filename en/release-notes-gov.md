## Dev Tools > Deploy > Release Notes

### September 10, 2024
#### Feature Updates
* Added support for deployment via NHN Cloud Agent to NHN Cloud Instance in the US (California) region
* Updated the version of the Jenkins plugin (version 1.1.3).
    * Made modifications so that, when binary group key is an empty value, the key is uploaded to the default binary group
    * Changed the endpoint's default value from https://api-tcd.cloud.toast.com to https://api-tcd.nhncloudservice.com

### July 9, 2024
#### Feature Updates
* Added sorting functionality within the **Binary Group** tab
* Improved the client binary deployment UI
    * You can send the download path to notification recipient groups
* Added the feature to check out the name by clicking **Executed By** on the **Deployment History** tab

### March 26, 2024
#### Feature Updates
* Added the feature to deploy via NHN Cloud Agent in addition to deployment via SSH when deploying NHN Cloud Instances
    * Deployment is available without assigning floating IPs to instances

### February 27, 2024
#### Feature Updates
* Improved the UI to modify server groups
* Added the run deployment API
* Added notification mail recipient settings
    * Added the feature to set email recipient address in Organization/Project Dashboard > Manage Notifications.

### January 23, 2024
#### Feature Updates
* Modified to set up the auto delete policy when creating or modifying binary groups

### November 28, 2023
#### Feature Updates
* Updated the version of the Jenkins Plugin (version 1.1.2).
    * Fixed an error that prevented Jenkins installed on a Windows environment from uploading to Deploy
* Changed the size limit for uploading binaries to APIs from 1 GB to 2 GB
#### Bug Fixes
* Fixed an error where resource file time information does not appear

### June 27, 2023
#### Bug Fixes
* Fixed a bug where binary deployment fails in Windows Server 2019 Standard

### May 30, 2023
#### Feature Updates
* Updated Jenkins Plugin (Version 1.1.1).
    * Fixed an error where uploading from Jenkins Agent node to Deploy fails

### April 25, 2023
#### Bug Fixes
* Fixed a bug where, when modifying deployment scenarios, more than 10 file lists are not loaded in the Select a file window
* Fixed an issue where, when resource file upload fails in the console, files are not retrieved and added the **Cancel Upload** button

### December 27, 2022
#### Feature Updates
* Displayed the number of rows of contents (Excel file) to download on the **Deployment History** tab.

### October 25, 2022
#### Feature Updates
* Added a feature to subdivide Deploy permission
* Modified to provide the notice of deployment execution only to users with Deploy ADMIN permission
* Changed the character limit for scenario and task names from 30 to 50
* Improved a feature to check whether execution can be performed
#### Bug Fixes
* Fixed a guide link error in Task and Create Artifacts
* Fixed an error where the Deploying status persists with Deploy not terminated

### August 23, 2022
* Changed the API endpoint's domain from gov-api-tcd.cloud.toast.com to api-tcd.gov-nhncloudservice.com.

### July 26, 2022
#### Feature Updates
* Added a message for binary upload failure
#### Bug Fixes
* Modified not to upload to the deleted binary groups

### April 26, 2022
#### Feature Updates
* Improved the query used when searching the artifact list
* Improved the service so that it waits until the execution is completed when running a Jenkins Pipeline job with Jenkins API build
* Removed filename restrictions for uploading a scenario

### January 25, 2022
#### New product release
* It is a service to provide convenience for deployment and installation.
* You can deploy software easily and quickly with a single click.
