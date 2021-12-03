## Dev Tools > Deploy > Release Notes

### December 28, 2021
#### Bug Fixes
* Fixed a guide link error in Task

### October 26, 2021
#### Bug Fixes
* Fixed a bug where non-payment users could use the service normally

### July 27, 2021
#### Feature Improvements
* Improved a feature to extract deployment history inquiry
    * Modified to include deployment history where only pre-run tasks exist
* Link with the instance disposal feature when Auto Scale service-integrated deployment fails
    * Scaled-out instances are disposed when deployment fails while scaling out in the Auto Scale service
    * The scale-out feature stops working when scale-out deployment fails three times or more
* Applied a security vulnerability patch

### June 29, 2021
#### Feature Updates
* Changed the **Deployment History** tab search conditions
    * Search deployment history by the server group and date of execution (starting date and ending date)
    * Limited the total search duration of date of execution to 1 year
* Added a feature to be used to download the deployment history result in an Excel file
    * Added an option to be used to download deployment histories except the ones without binary file

### March 23, 2021
#### Bug Fixes
* Fixed an issue where every member of a project would not be exposed on the client binary transfer target list

### February 23, 2021
#### Feature Updates
- Added Japanese translation to autoscale related features
#### Bug Fixes
* Fixed an error where autoscale deployment intermittently fails

### January 26, 2021
#### Bug Fixes
* Fixed 2020 data check errors of deployment records tab

### November 24, 2020
#### Feature Updates
* Added features for Auto Scale service-integrated deployment (excluding US region)
    * A feature to create Auto Scale type server groups and map the groups with scenarios
    * Custom deployment feature for Auto Scale group
* Added a feature to check deployment execution status
    * A feature to check whether the deployment is running before executing the deployment

### August 25, 2020
#### Feature Updates
* Displays time information at the start and end stamps of a log monitoring task
* Each TOAST service can be controlled according to permission

### April 28, 2020
#### Feature Updates
* Removed reference of HTTP resources within HTTPS page

### March 24, 2020
#### Feature Updates
* Added integration with TOAST Trail service
    * Enables users to check user events that occur on Deploy console through TOAST Trail

### February 25, 2020
#### Feature Updates
* Added the feature of default region setting for binary groups, when an artifact is created
    * Region (KR1/JP1) can be specified for binary groups when an artifact is created
* Added the feature of specifying default binary group for an artifact setting
    * Select from binary groups within artifact
#### Bug Fixes
* Fixed invalid binary group key setting for a binary task, when a scenario is uploaded

### December 24, 2019
#### Feature Updates
* Added Region for Binary Groups
    * To be specified when a binary is created
    * Download from the storage of an integrated region when downloading and deploying a binary
* Applied expiration time for client download pages

### September 24, 2019
#### Feature Updates
* Added a message for binary deployment failure due to inaccessible network

### July 23, 2019
#### Feature Updates
* Improved guide on plist parsing failure in Binary > iOS File Uploads
#### Feature Modification
* Adjusted the number of characters for SMS delivery so that it does not exceed specified size

### June 25, 2019
#### Bug Fixes
* Fixed a deployment error that occurs when binary version exceeds 100 characters when uploading binaries
* Fixed an error where the deleted binary download link could be accessed on the binary download page

### May 28, 2019
#### Feature Updates
* Added an artifact search feature

#### Bug Fixes
* Fixed a page error which occurs when a script is included in user command in Deployment History > View Result window

### April 23, 2019
#### Feature Updates
* Improved so that deployment progress is updated in real time in Deployment History tab > View Result popup

#### Bug Fixes
* Fixed an error where the task progress is not shown when deploying

### March 26, 2019
#### Features Updates
* Applied resource file upload size limit (1GB)

### February 26, 2019
#### Feature Updates
* Applied restrictions on editable resource size and content
    * Before: Unlimited size, all editable format files
    * After: 1MB size limit, only the formats that can be edited in Unicode are editable
* Increased task timeout time limit
    * Before: Up to 30 minutes
    * After: Up to 2 hours

#### Bug Fixes
* Fixed a malfunction of the Select All Server Groups checkbox when modifying a server group

### January 15, 2019
#### Feature Updates
* Added User Console Org authentication

### October 23, 2018
#### Added Features
* Enable deployment termination function after deployment re-entry (for deployer only)

#### Bug Fixes
* Fixed an issue where a file is created even if there is no file content in Resource tab > Add file

### August 28, 2018
#### Added Features
* Added deployment re-entry function
* Added a layer to guide page loading
* Binary upload API Ver 1.0 update
    * Operating simultaneously with previous versions

#### Bug Fixes
* Fixed the operation error of the item of authentication method at deployment
    * Added pem file required value check
    * Fixed an issue where the input type is not switched by clicking the password/pem switch radio button

#### Document Change
* Binary upload API Ver 1.0 update

### July 24, 2018
#### Added Features

* Added scenario Import / Export function
* Release restrictions on Client Type menu (all menus available)
* Added Phase property for server group equipment separator
    * Added confirmation step when deploying server with Phase Type of Product

### April 24, 2018
#### Added Features

* Added binary install page access control function of client application through password input

### February 22, 2018
#### New product release

* It is a service to provide convenience for deployment and installation.
* You can deploy software easily and quickly with a single click.
