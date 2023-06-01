# AS-Incident-IP-Matched-on-Watchlist

Author: Accelerynt

For any technical questions, please contact info@accelerynt.com  

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAccelerynt-Security%2FAS-Incident-IP-Matched-on-Watchlist%2Fmain%2Fazuredeploy.json)
[![Deploy to Azure Gov](https://aka.ms/deploytoazuregovbutton)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAccelerynt-Security%2FAS-Incident-IP-Matched-on-Watchlist%2Fmain%2Fazuredeploy.json)       

This playbook is intended to be run from a Microsoft Sentinel incident. If any IP entities are found to match those in a specified watchlist containing a list of subnets, a comment noting this match will be added to the incident and the incident will be tagged with a label of your choosing.

![IPMatchedOnWatchlist_Demo_1](Images/IPMatchedOnWatchlist_Demo_1.png)

![IPMatchedOnWatchlist_Demo_2](Images/IPMatchedOnWatchlist_Demo_2.png)
 
                                                                                                                                
#
### Requirements
                                                                                                                                     
The following items are required under the template settings during deployment: 

* A [Microsoft Sentinel Watchlist](https://github.com/Accelerynt-Security/AS-Incident-IP-Matched-on-Watchlist#create-a-microsoft-sentinel-watchlist) containing subnets is needed


# 
### Setup
                                                                                                                                     
#### Create a Microsoft Sentinel Watchlist:

If you already have a Microsoft Sentinel watchlist with subnet values, take note of the name and skip this step. 

Otherwise, navigate to the Microsoft Sentinel page and select a workspace:

https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/microsoft.securityinsightsarg%2Fsentinel

Under the "**Configuration**" section of the menu, click "**Watchlist**", then click "**Add new**".

![IPMatchedOnWatchlist_Create_Watchlist_1](Images/IPMatchedOnWatchlist_Create_Watchlist_1.png)

Fill out the required fields and take note of the value you use for "**Alias**" as this will be needed for deployment. Then click "**Next: Source**".

![IPMatchedOnWatchlist_Create_Watchlist_2](Images/IPMatchedOnWatchlist_Create_Watchlist_2.png)

The watchlist cannot be created without initial data. We have created a file with the necessary headers and an entry that can later be deleted from the watchlist once it has been updated with additional entries.

Upload the "**watchlist_initialize.csv**" included in this repository and select "**id**" as the search key. Then click "**Next: Review and create**".

![IPMatchedOnWatchlist_Create_Watchlist_3](Images/IPMatchedOnWatchlist_Create_Watchlist_3.png)

Review the information, then click "**Create**".
![IPMatchedOnWatchlist_Create_Watchlist_4](Images/IPMatchedOnWatchlist_Create_Watchlist_4.png)

Once your watchlist has been created, you can view the entries by clicking the watchlist name from the "**Overview**" page, and then clicking "**View in logs**".

![IPMatchedOnWatchlist_Create_Watchlist_5](Images/IPMatchedOnWatchlist_Create_Watchlist_5.png)

This will run a Kusto query for your watchlist and you should be able to see the initializing data that was just uploaded. Please note it may take a minute after the creation of your watchlist for the query to show results.

![IPMatchedOnWatchlist_Create_Watchlist_6](Images/IPMatchedOnWatchlist_Create_Watchlist_6.png)


#### Microsoft Sentinel Workspace Id:

Navigate to the Microsoft Sentinel page and select the same workspace as before:

https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/microsoft.securityinsightsarg%2Fsentinel

Under the "**Configuration**" section of the menu, click "**Settings**", then click the "**Workspace settings**" tab.

![IPMatchedOnWatchlist_Workspace_Id_1](Images/IPMatchedOnWatchlist_Workspace_Id_1.png)

Copy the value of the "**Workspace ID**" field and save it for deployment.

![IPMatchedOnWatchlist_Workspace_Id_2](Images/IPMatchedOnWatchlist_Workspace_Id_2.png)


#
### Deployment                                                                                                         
                                                                                                        
To configure and deploy this playbook:
 
Open your browser and ensure you are logged into your Microsoft Sentinel workspace. In a separate tab, open the link to our playbook on the Accelerynt Security GitHub Repository:

https://github.com/Accelerynt-Security/AS-Incident-IP-Matched-on-Watchlist

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAccelerynt-Security%2FAS-Incident-IP-Matched-on-Watchlist%2Fmain%2Fazuredeploy.json)
[![Deploy to Azure Gov](https://aka.ms/deploytoazuregovbutton)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAccelerynt-Security%2FAS-Incident-IP-Matched-on-Watchlist%2Fmain%2Fazuredeploy.json)                                             

Click the "**Deploy to Azure**" button at the bottom and it will bring you to the custom deployment template.

In the **Project Details** section:

* Select the "**Subscription**" and "**Resource Group**" from the dropdown boxes you would like the playbook deployed to.  

In the **Instance Details** section:   

* **Playbook Name**: This can be left as "**AS-Incident-IP-Matched-on-Watchlist**" or you may change it.  

* **Watchlist Name**: The name of the watchlist referenced in [Create a Microsoft Sentinel Watchlist](https://github.com/Accelerynt-Security/AS-Import-AD-Group-Users-to-MS-Watchlist#create-a-microsoft-sentinel-watchlist)

* **Workspace Id**: The Id of the Microsoft Sentinel workspace the watchlist was created in, referenced in [Microsoft Sentinel workspace Id](https://github.com/Accelerynt-Security/AS-Import-AD-Group-Users-to-MS-Watchlist#microsoft-sentinel-workspace-id)

* **Resource Group Name**: The name of the Microsoft Sentinel resource group the workspace is housed in

* **Incident Tag**: The name of the tag to add to the Microsoft Sentinel incident

Towards the bottom, click on "**Review + create**". 

![IPMatchedOnWatchlist_Deploy_1](Images/IPMatchedOnWatchlist_Deploy_1.png)

Once the resources have validated, click on "**Create**".

![IPMatchedOnWatchlist_Deploy_2](Images/IPMatchedOnWatchlist_Deploy_2.png)

The resources should take around a minute to deploy. Once the deployment is complete, you can expand the "**Deployment details**" section to view them.
Click the one corresponding to the Logic App.

![IPMatchedOnWatchlist_Deploy_3](Images/IPMatchedOnWatchlist_Deploy_3.png)

#
### Microsoft Sentinel Contributor Role

After deployment, you will need to give the system assigned managed identity the "**Microsoft Sentinel Contributor**" role. This will enable it to add comments to incidents. Navigate to the Log Analytics Workspaces page and select the same workspace the playbook is located in:

https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.OperationalInsights%2Fworkspaces

Select the "**Access control (IAM)**" option from the menu blade, then click "**Add role assignment**".

![IPMatchedOnWatchlist_Add_Contributor_Role_1](Images/IPMatchedOnWatchlist_Add_Contributor_Role_1.png)

Select the "**Microsoft Sentinel Contributor**" role, then click "**Next**".

![IPMatchedOnWatchlist_Add_Contributor_Role_2](Images/IPMatchedOnWatchlist_Add_Contributor_Role_2.png)

Select the "**Managed identity**" option, then click "**Select Members**". Under the subscription the logic app is located, set the value of "**Managed identity**" to "**Logic app**". Next, enter "**AS-Incident-IP-Matched-on-Watchlist**", or the alternative playbook name used during deployment, in the field labeled "**Select**". Select the playbook, then click "**Select**".

![IPMatchedOnWatchlist_Add_Contributor_Role_3](Images/IPMatchedOnWatchlist_Add_Contributor_Role_3.png)

Continue on to the "**Review + assign**" tab and click "**Review + assign**".

![IPMatchedOnWatchlist_Add_Contributor_Role_4](Images/IPMatchedOnWatchlist_Add_Contributor_Role_4.png)


**Alternatively**, if you wish to do this through PowerShell, run the following commands, replacing the managed identity object id and resource group name. You can find the managed identity object id on the Identity blade under Settings for the Logic App.

![IPMatchedOnWatchlist_Add_Contributor_Role_5](Images/IPMatchedOnWatchlist_Add_Contributor_Role_5.png)

You will not need to run the Install-Module if this has been done before. More documentation on this module can be found here:

https://learn.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-9.3.0

```powershell
Install-Module -Name Az
Connect-AzAccount
New-AzRoleAssignment -ObjectId <logic app managed identity object id> -RoleDefinitionName "Microsoft Sentinel Contributor" -ResourceGroupName "<logic app resource group name>"
```
