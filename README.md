Provision Azure Environment Resources from Uploaded VHD
=======================================================

            

**Description**


This runbook creates a number of Azure Environment Resources (in sequence): Azure Affinity Group, Azure Cloud Service, Azure Storage Account, Azure Storage Containers, and then Azure VM Image, and Azure VM all from an uploaded VHD.


**Requirements**


The creation of the Azure Enviornment Resources are based on the following input parameters: 'Project Name', 'VM Name', 'VM Instance Size' (and optionally 'Storage Account Name').


Before executing this runbook, make sure the following components are availabile:


  *  This runbook sample leverages organization id credential based authentication (Azure AD; instead of the Connect-Azure Runbook). Before using this runbook, you must create an Azure Active Directory user and allow that user to manage the Azure subscription
 you want to work against. You must also place this user's username / password in an Azure Automation credential asset.

You can find more information on configuring Azure so that Azure Automation can manage your Azure subscription(s) here: http://aka.ms/Sspv1l 

It does leverage an Automation Asset for the required Azure AD Credential. This example uses the following call to get this credential from the Asset store: 

     $Cred = Get-AutomationPSCredential -Name 'Azure AD Automation Account'
     $AzureSubscriptionName = Get-AutomationVariable -Name 'Primary Azure Subscription'


  *  The Upload of a VHD to a specified storage container mid-process. At this point in the process, the runbook will intentionally suspend and notify the user; after the upload, the user simply resumes the runbook and the rest of the creation process continues.

  *  Six more (6) Automation Assets (to be configured in the Assets tab). These are suggested, but not necessarily required. Replacing the 'Get-AutomationVariable' calls within this runbook with static or parameter variables is an alternative method. For this
 example though, the following dependencies exist:

  *  VARIABLES SET WITH AUTOMATION ASSETS:
        $AGLocation = Get-AutomationVariable -Name 'AGLocation'
        $GenericStorageContainerName = Get-AutomationVariable -Name 'GenericStorageContainer'
        $SourceDiskFileExt = Get-AutomationVariable -Name 'SourceDiskFileExt'
        $VMImageOS = Get-AutomationVariable -Name 'VMImageOS'
        $AdminUsername = Get-AutomationVariable -Name 'AdminUsername'
        $Password = Get-AutomationVariable -Name 'Password'



***Note     **The entire runbook is heavily checkpointed and can be run multiple times without resource recreation.*



**Runbook Contents**


The runbook's contents are displayed below:

 
 

        
    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.
