---
layout: post
title: "Connect to Azure DevOps Repository"
date: 2020-11-26 09:20:30 +0800
categories: jenkins azure devops
---

## Connect to Azure DevOps Repository

I was struggling to find the right set of instruction to connect Jenkins with Azure Devops Git Repository. Howevery after many attempts on getting Jenkins Build to fetch the git repository from Azure Devops worked.

The solution was rather simple then anything.

**Step 1.** Access https://[organization].visualstudio.com/\_usersSettings/tokens to generate a Personal Access Token (PAT) <br><br>
**Step 2.** Click on New Token <br>
![alt text](https://i.imgur.com/XPVShHM.jpg) <br><br>
**Step 3.** Enter the "Name" (something easier for you to identify where this token is being used), Expiration Date, and Code -> Read Permission and click on "Create"<br>
![alt text](https://i.imgur.com/X6kxp6J.jpg) <br><br>
**Step 4.** The confirmation message shows the PAT that will be only accessible here and would not be stored. So be sure to copy in a notepad.<br>
![alt text](https://i.imgur.com/CMeKWdM.jpg) <br><br>
**Step 5.** Next step is easy. Copy the clone url of the repository and add an arbitary username and PAT in the url (as shown below) <br>

https://`anything:[PAT]`@[organization].visualstudio.com/[project]/\_git/[repository]

**Step 6.** That's all we have to do. The URL is now ready to be configured in Jenkins
