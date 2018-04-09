# Setup your Azure subscription and Dynamics 365 for Operations environment for Test Drives

1.	Login to your Azure management portal
2.	Create a new tenant in AAD (if not available already)
      *    Navigate to https://portal.azure.com
      *    Click on + New ->  (Search for Azure Active Directory)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub1.jpg)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub2.jpg)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub3.jpg)

3. 	Register an application in azure. We will use this application to perform operations on your Dynamics 365 instance including adding and removing users etc. 

      *    Navigate to the newly created directory or already existing directory and select Azure Active directory in the filter pane.
      *    Search “App registrations” and click on “Add”
      *    Provide an application name.
      *    Provide an application name.
      *    Select the Type of as “Web app/ API”
      *    Provide any sign-on URL - Ex- http://localhost (It can be changed or updated as per need later on)
      *    Click Create. It will take a while to create the application. Wait for sometime.
      *    Click on "Settings" to configure the application.
      *    Go to  Properties -> Set the application as multi-tenant and hit Save. Get more details about Multi-Tenant applicaton using [link](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application).
      *    Generate a client secret to access the application. Under keys, add a Key Description, set the duration to two years or as appropriate. Do remember to update this before the key expires, else your test drives will be broken. 
      *    Click Save. This should generate the ClientSecret. Copy this value as it will be hidden afterwards. Generate a new one in case you lost the current one. 
      *    Update the Required Permissions for the application. By default the application will have "Windows Azure Active Directory".
      *    Click Add followed by Select an API
      *    Find Microsoft Graph from the available list of API's and press Select
      *    Select permissions for: "Read and write directory data", "Read directory data" under Application Permission and press Done.
      *    Click on **"Grant Permissions"** to sync the added/updated permission.
      *    Hit Save.

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub4.jpg)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub5.jpg)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/SetupSub6.jpg)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/TestDrive_GrantPermission.png)

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/TestDriveGrantPermissions.PNG)

4. Add Service Principal role to application to allow delete access during Deprovisioning.
    * Install-Module MSOnline  (run this command if MSOnline is not installed)
    * Connect-MsolService (Will show a popup window to login. Login with newly created org tenant)
    * $applicationId = "<YOUR_APPLICATION_ID>"
    * $sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
    * Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
 
 ![](https://github.com/Microsoft/AppSource/blob/patch-1/Images/Connect_MsolService.PNG)

5. Now we need to add the above app to Dynamics 365 for operations in order to enable the app to manage users
      *    Navigate to your Dynamics 365 for Operations instance.
      *    Click on the Hamburger menu at the left top corner.
      *    Click on System Administration.
      *    Click on Azure Active Directory applications.
      *    Click on + New
      *    Enter in the Client Id of the Azure AD app that is going to perform the on-behalf-of actions.
      *    The user Id on whose behalf the actions will be performed (typically the System Admin of the instance or a user who has privileges to add other users).

![](https://github.com/Azure/AzureTestDrive/blob/master/AzureTestDriveImages/Dynamics365OperationsAddApp.png)