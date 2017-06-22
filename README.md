# Build a Telco Conversation Dashboard application using Bluemix & Watson Services

## Overview

This application demonstrates how the Bluemix Conversation service can be used to build a simple on-board telco chat application.

* We will learn and understand how to integrate the Conversation, Discovery and Cloudant services.

Following services are used from Bluemix:
1. Watson Conversation
2. Watson Cloudant
3. Watson Discovery

See the app [demo](http://conversation-demo.mybluemix.net/)

For more information about Conversation, see the [detailed documentation](https://www.ibm.com/watson/developercloud/doc/conversation/index.html).

## How the app works

The app interface is designed and trained for chatting with the telco customers. The chat interface is on the left, and the JSON that the JavaScript code receives from the server is on the right. Your questions and commands are run against a small set of sample data trained with intents like these:

    view profile
    view account
    edit email

These intents help the system to understand variations of questions and commands that you might submit.

Example commands that can be executed by the Conversation service are:

    view profile
    edit emailId

## Technical Architecture - Telco Chatbot
<b>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="readme_images/Telco-Architecture.png" width="800" height="350" align="center">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


- The user writes a query about a service to the chatbot
- The orchestrator accepts query, saves the user context & starts a REST call with the Cognitive APIs.
- Depending on the nature of the query (long /Short tail), the answer is provided by either Watson Conversation API or Watson Discovery Services

## Pre-requisities

Ensure that you have a [Bluemix account](https://console.ng.bluemix.net/registration/)

### Deploy the app and explore the conversation flow

1. Select "Deploy to Bluemix" icon below.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/DevConnect2017/TelcoChatBot)

2. This action deploys 1 application and 2 services. You can view this on your Bluemix Dashboard.

3. Log in with your existing Bluemix account or Sign up

4. Name your app and select your REGION, ORGANIZATION and SPACE. Then select DEPLOY.

![](readme_images/WithToolchain.png)

5. The deploy performs two actions:  

    * deploys the app run time
    * creates a Conversation and Cloudant service instances
    * connects to the already created Discovery service

6. To see the status of your app, you can click "Delivery Pipeline".

![](readme_images/ToolChain3.png)

7. The deployment can take some time. 

8. Deployment of the app might FAIL. The reason for the failure is that we have to configure the cloudant database.

To configure the cloudant database please follow the below steps:

### Cloudant

1. Click on "cloudant-db" from the Bluemix Dashboard. In the landing page, click on Launch button.

2. Click on Databases from left hand navigation. Click on "Create Database" from top right corner.

3. Give a name to the database, like <b>telco-users</b>

4. To load demo / sample data to the database, two versions of data are available under resources folder: *telco-users-cloudant.json* and *telco-users-cloudant.txt*

5. Install couchimport tool to upload / download data to cloudant using the following commnands:

Open a bash terminal and run the following snippet:
sudo npm install -g couchimport  

To get the URL -- Got to the App section --> Runtime --> Environment Variable --> Under cloudant service check for the URL. 
export COUCH_URL="https://username:password@xxx-bluemix.cloudant.com"  

export COUCH_DATABASE="telco-users"  

cat telco-users-cloudant.txt | couchimport --db telco-users



### Importing a workspace

To use the app you're creating, you need to add a workspace to your Conversation service. A workspace is a container for all the artifacts that define the behavior of your service (ie: intents, entities and chat flows).

For more information on workspaces, see the full  [Conversation service documentation](https://www.ibm.com/watson/developercloud/doc/conversation/index.html).

1. Navigate to the Bluemix dashboard, select the Conversation service that you created

2. Go to the **Manage** menu item and select **Launch Tool**. This opens a new tab in your browser, where you are prompted to login if you have not done so before. Use your Bluemix credentials.

3. Download the exported file that contains the Workspace contents by clicking [exported JSON file](https://github.com/DevConnect2017/TelcoChatBot)
    - navigate to the resources folder to find `telcobot-conversation-workspace.json` file
    - click on the "Raw" link which will open a browser
    - Select "Save As" and save the file as a json

4. Select the import icon: ![](readme_images/importGA.PNG). Browse to (or drag and drop) the JSON file. Choose to import **Everything(Intents, Entities, and Dialog)**. Then select **Import** to finish importing the workspace.

5. Refresh your browser. A new workspace tile is created in the tool. Select the _menu_ button within the workspace tile, then select **View details**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Workpsace Details](readme_images/details.PNG)

In the Details UI, copy the 36 character UNID **ID** field. This is the **WORKSPACE_ID**. Retain this for the following step where we will create an environment variable. This varibale will be the handle for the application to refer to the Conversation Service that we created.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![](readme_images/workspaceid.PNG)

6. Go to DevOps screen and click on "Eclipse Orion Web IDE" and open in new window as shown below:

![](readme_images/OrionEditor.png)

7. In app.js line number 45 edit the workspace ID as shown below

![](readme_images/WorkspaceIDEdit.png)

8. Commit the changes by entering the comments as follows:

![](readme_images/GitCommit.png)

9. And then it will prompt for name and email id as follows:
![](readme_images/CommitGitChanges.png)

9. Click on the 'Push' icon on the left:
![](readme_images/GitPushDone.png)

10. Goto the DevOps --> Delivery pipeline of your project to see the redployment of the app:

![](readme_images/ReDeployment.png)

## Running the app

1. Launch the application from bluemix dashboard

2. Enter an username, say, ajay to login and see the chat bot working


### Troubleshooting in Bluemix

#### In the Classic Experience:

- Log in to Bluemix, you'll be taken to the dashboard
- Navigate to the the application you previously created
- Select **Logs**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](readme_images/logs.PNG)

- If you want, filter the LOG TYPE by "APP"

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](readme_images/filter.PNG)

#### In the new Bluemix:

- Log in to Bluemix, you'll be taken to the dashboard
- Select **Compute**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](readme_images/compute.PNG)

- Select the application you previously created
- Select **Logs**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](readme_images/logs1.PNG)

- If you want, filter the Log Type by selecting the drop-down and selecting **Application(APP)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](readme_images/filter1.PNG)

## License

This sample code is licensed under Apache 2.0. Full license text is available in [LICENSE](LICENSE).


## Open Source @ IBM

Find more open source projects on the [IBM Github Page](http://ibm.github.io/).
