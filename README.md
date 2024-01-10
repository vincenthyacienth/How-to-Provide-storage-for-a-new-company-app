# How To Provide Storage For A New Company App
## Lets try this exercise;
The company is designing and developing a new app. Developers need to ensure the storage is only accessed using keys and managed identities. The developers would like to use role-based access control. To help with testing, protected immutable storage is needed.
## Exercise Instructions
## Create The Storage Account And Managed Identity
1. Provide a storage account for the web app.
- In the portal, search for and select Storage accounts.
- Select + Create.
- For Resource group select Create new. Give your resource group a name and select OK to save your changes.
- Provide a Storage account name. Ensure the name is unique and meets the naming requirements.
- Review, and then Create the storage account.
- Wait for the resource to deploy.
2. Provide a managed identity for the web app to use. 
- Search for and select Managed identities.
- Select Create.
  - Select your resource group.
  - Give your managed identity a name.
- Select Review and create, and then Create.
3. Assign the correct permissions to the managed identity. The identity only needs to read and list containers and blobs.
- Search for and select your storage account.
- Select the Access Control (IAM) blade.
- Select Add role assignment (center of the page).
- On the Job functions roles page, search for and select the Storage Blob Data Reader role.
- On the Members page, select Managed identity.
- Select Select members, in the Managed identity drop-down select User-assigned managed identity.
- Select the managed identity you created in the previous step.
- Click Select and then Review + assign the role.
- Select Review + assign a second time to add the role assignment.
- Your storage account can now be accessed by a managed identity with the Storage Data Blob Reader permissions.
## Secure Access To The Storage Account With A Key Vault And Key
1. To create the key vault and key needed for this part of the lab, your user account must have Key Vault Administrator permissions. 
- In the portal, search for and select Resource groups.
- Select your resource group, and then the Access Control (IAM) blade.
- Select Add role assignment (center of the page).
- On the Job functions roles page, search for and select the Key Vault Administrator role.
- On the Members page, select User, group, or service principal.
- Select Select members.
- Search for and select your user account. Your user account is shown in the top right of the portal.
- Click Select and then Review + assign.
- Select Review + assign a second time to add the role assignment.
- You are now ready to continue with the lab.
2. Create a key vault to store the access keys.
- In the portal, search for and select Key vaults.
- Select Create.
- Select your resource group.
- Provide the name for the key vault. The name must be unique.
- Select Review + create.
- Wait for the validation checks to complete and then select Create.
- After the deployment, select Go to resource.
- On the Overview blade ensure both Soft-delete and Purge protection are enabled.
3. Create a customer-managed key in the key vault.
- In your key vault, in the Objects section, select the Keys blade.
- Select Generate/Import and Name the key.
- Take the defaults for the rest of the parameters, and Create the key.
## Configure The Storage Account To Use The Customer Managed Key In The Key Vault
1. Before you can complete the next steps, you must assign the Key Vault Crypto Service Encryption User role to the managed identity.
- In the portal, search for and select Resource groups.
- Select your resource group, and then the Access Control (IAM) blade.
- Select Add role assignment (center of the page).
- On the Job functions roles page, search for and select the Key Vault Crypto Service Encryption User role.
- On the Members page, select Managed identity.
- Select Select members, in the Managed identity drop-down select User-assigned managed identity.
- Select your managed identity.
- Click Select and then Review + assign.
- Select Review + assign a second time to add the role assignment.
2. Configure the storage account to use the customer managed key in your key vault.
- Return to your the storage account.
- In the Security + networking section, selet the Encryption blade.
- Select Customer-managed keys.
- Select a key vault and key. Select your key vault and key.
- Select to confirm your choices.
- Ensure the Identity type is User-assigned.
- Select an identity.
- Select your managed identity then select Add.
- Save your changes.
- If you receive an error that your identity does not have the correct permissions, wait a minute and try again.
## Configure An Time-based Retention Policy And An Encryption Scope.
1. The developers require a storage container where files can’t be modified, even by the administrator.
- Navigate to your storage account.
- In the Data storage section, select the Containers blade.
- Create a container called hold. Take the defaults. Be sure to Create the container.
- Upload a file to the container.
- In the Settings section, select the Access policy blade.
- In the Immutable blob storage section, select + Add policy.
- For the Policy type, select time-based retention.
- Set the Retention period to 5 days.
- Be sure to Save your changes.
- Try to delete the file in the container.
- Verify you are notified failed to delete blobs due to policy.
2. The developers require an encryption scope that enables infrastructure encryption. 
- Navigate back to your storage account.
- In the Security + networking blade, select Encryption.
- In the Encryption scopes tab, select Add.
- Give your encryption scope a name.
- The Encryption type is Microsoft-managed key.
- Set Infrastructure encryption to Enable.
- Notice the warning that enabling infrastructure encryption can not be changed after the scope is created.
- Create the encryption scope.
- Return to your storage account and create a new container.
- Notice on the New container page, there is the Name and Public access level.
- Notice in the Advanced section you can select the Encryption scope you created and apply it to all blobs in the container.
