<!-- pre-align:aligned sig=466fe76e9c00 -->

## Dev Tools > Deploy > Release Notes

### August 11, 2026
#### Added Features
* Added the API v2.1 List Scenarios API
    * You can retrieve the ID and name of scenarios mapped to a server group.

### April 14, 2026 { #april-14-2026 }
<a id="added-features"></a>

#### Added Features
* Added the API v2.1 Guide
    * Applied the User Access Key Token (Bearer) authentication method.
* Added information retrieval APIs
    * API v2.0 and v2.1 now provide APIs to list artifacts, list server groups, list binary groups, retrieve deployment history, and list binaries.

<a id="april-29-2025"></a>

### April 29, 2025 { #april-29-2025 }
<a id="april-29-2025-added-features"></a>

#### Added Features
* Added Deployment Execution API Version 2.0
    * You can now run deployments on Auto Scale server groups as well.

<a id="feature-updates"></a>

#### Feature Updates
* Changed the Cloud Agent artifact Command Type to support creating Auto Scale server groups
* Changed the binary upload size limit via API from 2 GB to 4 GB

<a id="march-25-2025"></a>

### March 25, 2025 { #march-25-2025 }
<a id="march-25-2025-feature-updates"></a>

#### Feature Updates
* Changed to sort by name when exposing binary groups

<a id="january-21-2025"></a>

### January 21, 2025 { #january-21-2025 }
<a id="bug-fixes"></a>

#### Bug Fixes
* Fixed an error of moving to the error page when the scenario content is incorrect
* Fixed to enter ID rather than email as an executor when calling Run Deployment API with IAM account

<a id="november-26-2024"></a>

### November 26, 2024 { #november-26-2024 }
<a id="november-26-2024-feature-updates"></a>

#### Feature Updates
* Changed deployment history to only be viewable for the last 5 years
  
<a id="september-10-2024"></a>

### September 10, 2024 { #september-10-2024 }
<a id="september-10-2024-feature-updates"></a>

#### Feature Updates
* Added support for deployment via NHN Cloud Agent to NHN Cloud Instance in the US (California) region
* Jenkins plugin update (version 1.1.3)
    * Made modifications so that, when binary group key is an empty value, the key is uploaded to the default binary group. 
    * Changed the endpoint's default value from https://api-tcd.cloud.toast.com to https://api-tcd.nhncloudservice.com.

<a id="july-9-2024"></a>

### July 9, 2024 { #july-9-2024 }
<a id="july-9-2024-feature-updates"></a>

#### Feature Updates
* Added sorting functionality within the **Binary Group** tab
* Improved the client binary deployment UI
    * You can send the download path to notification recipient groups.
* Added the feature to check out the name by clicking **Executed By** on the **Deployment History** tab

<a id="june-11-2024"></a>

### June 11, 2024 { #june-11-2024 }
<a id="june-11-2024-bug-fixes"></a>

#### Bug Fixes
* Fixed an issue where autoscale server groups cannot be fixed

<a id="march-26-2024"></a>

### March 26, 2024 { #march-26-2024 }
<a id="march-26-2024-feature-updates"></a>

#### Feature Updates
* Added the feature to deploy via NHN Cloud Agent in addition to deployment via SSH when deploying NHN Cloud Instances
    * Deployment is available without assigning floating IPs to instances.

<a id="february-27-2024"></a>

### February 27, 2024 { #february-27-2024 }
<a id="february-27-2024-feature-updates"></a>

#### Feature Updates
* Improved the UI to modify server groups
* Added the run deployment API
* Added notification mail recipient settings
    * Added the feature to set email recipient address in Organization/Project Dashboard > Manage Notifications.

<a id="january-23-2024"></a>

### January 23, 2024 { #january-23-2024 }
<a id="january-23-2024-feature-updates"></a>

#### Feature Updates
* Modified to set up the auto delete policy when creating or modifying binary groups

<a id="november-28-2023"></a>

### November 28, 2023 { #november-28-2023 }
<a id="november-28-2023-feature-updates"></a>

#### Feature Updates
* Jenkins Plugin update (version 1.1.2).
    * Fixed an error that prevented Jenkins installed on a Windows environment from uploading to Deploy.
* Changed the size limit for uploading binaries to APIs from 1 GB to 2 GB
<a id="november-28-2023-bug-fixes"></a>

#### Bug Fixes 
* Fixed an error where resource file time information does not appear

<a id="june-27-2023"></a>

### June 27, 2023 { #june-27-2023 }
<a id="june-27-2023-bug-fixes"></a>

#### Bug Fixes
* Fixed a bug where binary deployment fails in Windows Server 2019 Standard

<a id="may-30-2023"></a>

### May 30, 2023 { #may-30-2023 }
<a id="may-30-2023-feature-updates"></a>

#### Feature Updates
* Jenkins Plugin update (Version 1.1.1)
    * Fixed an error where uploading from Jenkins Agent node to Deploy fails.

<a id="april-25-2023"></a>

### April 25, 2023 { #april-25-2023 }
<a id="april-25-2023-bug-fixes"></a>

#### Bug Fixes
* Fixed a bug where, when modifying deployment scenarios, more than 10 file lists are not loaded in the Select a file window
* Fixed an issue where, when resource file upload fails in the console, files are not retrieved and added the **Cancel Upload** button

<a id="december-27-2022"></a>

### December 27, 2022 { #december-27-2022 }
<a id="december-27-2022-feature-updates"></a>

#### Feature Updates
* Displayed the number of rows of contents (Excel file) to download on the **Deployment History** tab.

<a id="october-25-2022"></a>

### October 25, 2022 { #october-25-2022 }
<a id="october-25-2022-feature-updates"></a>

#### Feature Updates
* Improved a feature to check whether execution can be performed
* Modified not to execute the scenarios without tasks

<a id="october-04-2022"></a>

### October 04, 2022 { #october-04-2022 }
<a id="october-04-2022-feature-updates"></a>

#### Feature Updates
* Added a feature to subdivide Deploy permission
* Modified to provide the notice of deployment execution only to users with Deploy ADMIN permission
* Changed the character limit for scenario and task names from 30 to 50
<a id="october-04-2022-bug-fixes"></a>

#### Bug Fixes
* Fixed a guide link error in Task and Create Artifacts
* Fixed an error where the Deploying status persists with Deploy not terminated

<a id="august-23-2022"></a>

### August 23, 2022 { #august-23-2022 }
* Changed the API endpoint's domain from api-tcd.cloud.toast.com to api-tcd.nhncloudservice.com.

<a id="july-26-2022"></a>

### July 26, 2022 { #july-26-2022 }
<a id="july-26-2022-feature-updates"></a>

#### Feature Updates
* Added a message for binary upload failure 
<a id="july-26-2022-bug-fixes"></a>

#### Bug Fixes
* Modified not to upload to the deleted binary groups

<a id="april-26-2022"></a>

### April 26, 2022 { #april-26-2022 }
<a id="april-26-2022-feature-updates"></a>

#### Feature Updates
* Improved the service so that it waits until the execution is completed when running a Jenkins Pipeline job with Jenkins API build
* Removed filename restrictions for uploading a scenario
<a id="april-26-2022-bug-fixes"></a>

#### Bug Fixes
* Fixed a bug in Windows Server log monitoring

<a id="march-29-2022"></a>

### March 29, 2022 { #march-29-2022 }
<a id="march-29-2022-feature-updates"></a>

#### Feature Updates
* Improved the query used when searching the artifact list

<a id="january-25-2022"></a>

### January 25, 2022 { #january-25-2022 }
<a id="january-25-2022-feature-updates"></a>

#### Feature Updates
* Integration with the CloudTrail service
    * In CloudTrail, you can check the **Execute Autoscale Deployment** user event that occurred in the Deploy console.

<a id="december-28-2021"></a>

### December 28, 2021 { #december-28-2021 }
<a id="december-28-2021-bug-fixes"></a>

#### Bug Fixes
* Fixed a guide link error in Task

<a id="october-26-2021"></a>

### October 26, 2021 { #october-26-2021 }
<a id="october-26-2021-bug-fixes"></a>

#### Bug Fixes
* Fixed a bug where non-payment users could use the service normally

<a id="july-27-2021"></a>

### July 27, 2021 { #july-27-2021 }
<a id="july-27-2021-feature-updates"></a>

#### Feature Updates
* Improved a feature to extract deployment history inquiry
    * Modified to include deployment history where only pre-run tasks exist.
* Link with the instance disposal feature when Auto Scale service-integrated deployment fails
    * Scaled-out instances are disposed when deployment fails while scaling out in the Auto Scale service.
    * The scale-out feature stops working when scale-out deployment fails three times or more.
* Applied a security vulnerability patch

<a id="june-29-2021"></a>

### June 29, 2021 { #june-29-2021 }
<a id="june-29-2021-feature-updates"></a>

#### Feature Updates
* Changed the **Deployment History** tab search conditions
    * Search deployment history by the server group and date of execution (starting date and ending date).
    * Limited the total search duration of date of execution to 1 year.
* Added a feature to be used to download the deployment history result in an Excel file
    * Added an option to be used to download deployment histories except the ones without binary file.

<a id="march-23-2021"></a>

### March 23, 2021 { #march-23-2021 }
<a id="march-23-2021-bug-fixes"></a>

#### Bug Fixes
* Fixed an issue where every member of a project would not be exposed on the client binary transfer target list

<a id="february-23-2021"></a>

### February 23, 2021 { #february-23-2021 }
<a id="february-23-2021-feature-updates"></a>

#### Feature Updates
- Added Japanese translation to autoscale related features
<a id="february-23-2021-bug-fixes"></a>

#### Bug Fixes
* Fixed an error where autoscale deployment intermittently fails

<a id="january-26-2021"></a>

### January 26, 2021 { #january-26-2021 }
<a id="january-26-2021-bug-fixes"></a>

#### Bug Fixes
* Fixed 2020 data check errors of deployment records tab

<a id="november-24-2020"></a>

### November 24, 2020 { #november-24-2020 }
<a id="november-24-2020-feature-updates"></a>

#### Feature Updates
* Added features for Auto Scale service-integrated deployment (excluding US region)
    * A feature to create Auto Scale type server groups and map the groups with scenarios.
    * Custom deployment feature for Auto Scale group.
* Added a feature to check deployment execution status
    * A feature to check whether the deployment is running before executing the deployment.

<a id="august-25-2020"></a>

### August 25, 2020 { #august-25-2020 }
<a id="august-25-2020-feature-updates"></a>

#### Feature Updates
* Displays time information at the start and end stamps of a log monitoring task
* Each TOAST service can be controlled according to permission

<a id="april-28-2020"></a>

### April 28, 2020 { #april-28-2020 }
<a id="april-28-2020-feature-updates"></a>

#### Feature Updates
* Removed reference of HTTP resources within HTTPS page

<a id="march-24-2020"></a>

### March 24, 2020 { #march-24-2020 }
<a id="march-24-2020-feature-updates"></a>

#### Feature Updates
* Added integration with TOAST Trail service
    * Enables users to check user events that occur on Deploy console through TOAST Trail.

<a id="february-25-2020"></a>

### February 25, 2020 { #february-25-2020 }
<a id="february-25-2020-feature-updates"></a>

#### Feature Updates
* Added the feature of default region setting for binary groups, when an artifact is created
    * Region (KR1/JP1) can be specified for binary groups when an artifact is created.
* Added the feature of specifying default binary group for an artifact setting
    * Select from binary groups within artifact.
<a id="february-25-2020-bug-fixes"></a>

#### Bug Fixes
* Fixed invalid binary group key setting for a binary task, when a scenario is uploaded

<a id="december-24-2019"></a>

### December 24, 2019 { #december-24-2019 }
<a id="december-24-2019-feature-updates"></a>

#### Feature Updates
* Added Region for Binary Groups
    * To be specified when a binary is created.
    * Download from the storage of an integrated region when downloading and deploying a binary.
* Applied expiration time for client download pages

<a id="september-24-2019"></a>

### September 24, 2019 { #september-24-2019 }
<a id="september-24-2019-feature-updates"></a>

#### Feature Updates
* Added a message for binary deployment failure due to inaccessible network

<a id="july-23-2019"></a>

### July 23, 2019 { #july-23-2019 }
<a id="july-23-2019-feature-updates"></a>

#### Feature Updates
* Improved guide on plist parsing failure in Binary > iOS File Uploads
<a id="feature-modification"></a>

#### Feature Modification
* Adjusted the number of characters for SMS delivery so that it does not exceed specified size

<a id="june-25-2019"></a>

### June 25, 2019 { #june-25-2019 }
<a id="june-25-2019-bug-fixes"></a>

#### Bug Fixes
* Fixed a deployment error that occurs when binary version exceeds 100 characters when uploading binaries
* Fixed an error where the deleted binary download link could be accessed on the binary download page

<a id="may-28-2019"></a>

### May 28, 2019 { #may-28-2019 }
<a id="may-28-2019-feature-updates"></a>

#### Feature Updates
* Added an artifact search feature

<a id="may-28-2019-bug-fixes"></a>

#### Bug Fixes
* Fixed a page error which occurs when a script is included in user command in Deployment History > View Result window

<a id="april-23-2019"></a>

### April 23, 2019 { #april-23-2019 }
<a id="april-23-2019-feature-updates"></a>

#### Feature Updates
* Improved so that deployment progress is updated in real time in Deployment History tab > View Result popup

<a id="april-23-2019-bug-fixes"></a>

#### Bug Fixes
* Fixed an error where the task progress is not shown when deploying

<a id="march-26-2019"></a>

### March 26, 2019 { #march-26-2019 }
<a id="features-updates"></a>

#### Features Updates
* Applied resource file upload size limit (1GB)

<a id="february-26-2019"></a>

### February 26, 2019 { #february-26-2019 }
<a id="february-26-2019-feature-updates"></a>

#### Feature Updates
* Applied restrictions on editable resource size and content
    * Before: Unlimited size, all editable format files
    * After: 1MB size limit, only the formats that can be edited in Unicode are editable
* Increased task timeout time limit
    * Before: Up to 30 minutes
    * After: Up to 2 hours

<a id="february-26-2019-bug-fixes"></a>

#### Bug Fixes
* Fixed a malfunction of the Select All Server Groups checkbox when modifying a server group

<a id="january-15-2019"></a>

### January 15, 2019 { #january-15-2019 }
<a id="january-15-2019-feature-updates"></a>

#### Feature Updates
* Added User Console Org authentication

<a id="october-23-2018"></a>

### August 28, 2018 { #october-23-2018 }
<a id="october-23-2018-added-features"></a>

#### Added Features
* Enabled the deployment termination feature after re-entry into deployment (deployers only)

<a id="october-23-2018-bug-fixes"></a>

#### Bug Fixes
* Fixed an issue where a file was created even when no file content was provided in the Resources tab > Add File

<a id="august-28-2018"></a>

### August 28, 2018 { #august-28-2018 }
<a id="august-28-2018-added-features"></a>

#### Added Features
* Deployment re-entry feature
* Added a layer to guide page loading
* Updated Binary Upload API Ver1.0
    * Operates simultaneously with the previous version.

<a id="august-28-2018-bug-fixes"></a>

#### Bug Fixes
* Fixed an authentication method item behavior error during deployment
    * Added a required value check for pem files.
    * Fixed an issue where the input type was not switched when clicking the password/pem toggle radio button.

<a id="document-change"></a>

#### Document Changes
* Updated Binary Upload API Ver1.0

<a id="july-24-2018"></a>

### July 24, 2018 { #july-24-2018 }
<a id="july-24-2018-added-features"></a>

#### Added Features

* Added scenario Import/Export feature
* Removed menu usage restrictions for Client Type (all menus are now available)
* Added Phase attribute to distinguish server group equipment
    * Added a confirmation step when deploying servers with a Phase Type of Product.

<a id="april-24-2018"></a>

### April 24, 2018 { #april-24-2018 }
<a id="april-24-2018-added-features"></a>

#### Added Features

* Added the feature to restrict access to the binary installation page of a client application using a password

<a id="february-22-2018"></a>

### February 22, 2018 { #february-22-2018 }
<a id="new-product-release"></a>

#### New product release

* It is a service to provide convenience for deployment and installation.
* You can deploy software easily and quickly with a single click.
