# -Key-Vault-Implementing-Secure-Data-by-setting-up-Always-Encrypted-

This repository contains a security proof-of-concept (POC) focused on implementing Azure SQL Database Always Encrypted functionality. The project demonstrates how to protect sensitive data at rest and in transit by leveraging Azure Key Vault for mandatory key and secret storage. To enhance security posture, the application is registered in Microsoft Entra ID.

# Objective

- Infrastructure Deployment: Automate the setup of a base environment using Azure Resource Manager (ARM) templates.
- Key Management: Configure Azure Key Vault to securely store and manage cryptographic keys and secrets.
- Database Encryption: Implement "Always Encrypted" on specific SQL Database columns to ensure data remains encrypted even from database administrators
- Secure Access: Use Microsoft Entra ID (formerly Azure AD) for application identity and secure resource access.

# Architecture & Components

- Azure SQL Database: Hosts the data with Always Encrypted enabled on sensitive columns (e.g., SSN, BirthDate).
- Azure Key Vault: Stores the Column Master Key (CMK) and application secrets.
- Azure Virtual Machine: Acts as a management and application host, pre-installed with Visual Studio 2019 and SQL Server Management Studio (SSMS) 19.
- Microsoft Entra ID: Provides identity-based authentication for the application to access Key Vault.

# Skills Learned

1. Cryptographic Key Management: Gained experience in creating and configuring an Azure Key Vault to securely store and manage cryptographic keys and secrets.
2. Database Security Implementation: Learned how to implement Always Encrypted functionality on specific Azure SQL Database columns, such as SSN and BirthDate, to protect sensitive data from unauthorized access, including database administrators.
3. Identity and Access Management (IAM): Developed skills in registering applications within Microsoft Entra ID (formerly Azure AD) to enhance security posture through managed identities and secure resource access.
4. Infrastructure as Code (IaC): Practiced deploying complex base environments using Azure Resource Manager (ARM) templates to automate the provisioning of Virtual Machines and SQL Databases.
5. Network Security Configuration: Learned to manage access to cloud resources by configuring Server Firewalls and setting specific firewall rules to allow secure connections from management environments
6. Cloud Administration & Tooling: Gained proficiency in using administrative tools such as the Azure Portal, Cloud Shell (PowerShell), SQL Server Management Studio (SSMS) 19, and Visual Studio 2019 for managing and building cloud-native applications.

# Tools Used
- Infrastructure & Cloud Shell Commands
  - ARM Template Deployment: Resources were provisioned using two primary JSON template files:
    - az-500-10_azuredeploy.json: Deployed the base infrastructure, including the Virtual Machine and SQL environment.
    - az-500-10_DB.json: Specifically handled the deployment and configuration of the Azure SQL Database.
  - PowerShell: Used within the Azure Cloud Shell to create and configure the Key Vault.
    - New-AzKeyVault: While the full string is partially truncated in the snippets, the report specifies using a PowerShell session to create the Azure Key Vault within a designated resource group.

- Database (SQL) Commands SQL commands were executed via SQL Server Management Studio (SSMS) to manage tables and verify the "Always Encrypted" state:
  - Table Creation: Commands were used to create a Patients table with sensitive columns
  - Data Verification Query: To prove data was encrypted at rest, the following query was run:
     SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
  - Decryption Verification: A targeted query was used in the application console to verify that authorized users could see decrypted data by searching for a specific SSN (e.g., 999-99-0003).

- Application & Management Tools
  - Visual Studio 2019: Used to build and run the data-driven console application (program.cs).
  - Remote Desktop Protocol (RDP): Used to connect to the management VM (az500-10-vm1) for administrative tasks
  - App Registration: Commands and workflows were used within Microsoft Entra ID to register the application and generate security keys (e.g., Key1) for authentication.
# Steps
Lab scenario 
You have been asked to create a proof of concept application that makes use of the Azure SQL Database 
support for Always Encrypted functionality. All of the secrets and keys used in this scenario should be 
stored in Key Vault. The application should be registered in Microsoft Entra ID in order to enhance its 
security posture. To accomplish these objectives, the proof of concept should include: 
• Creating an Azure Key Vault and storing keys and secrets in the vault. 
• Create a SQL Database and encrypting content of columns in database tables by using Always 
Encrypted. 
For all the resources in this lab, we are using the East US region. Verify with your instructor this is the 
region to use for class. 
To keep the focus on the security aspects of Azure, related to building this proof of concept, you will start 
from an automated ARM template deployment, setting up a Virtual Machine with Visual Studio 2019 and 
SQL Server Management Studio 19. 
Lab objectives 
In this lab, you will complete the following exercises: 
• Exercise 1: Deploy the base infrastructure from an ARM template 
• Exercise 2: Configure the Key Vault resource with a key and a secret 
• Exercise 3: Configure an Azure SQL database and a data-driven application 
• Exercise 4: Demonstrate the use of Azure Key Vault in encrypting the Azure SQL database 
Instructions 
Lab files: 
• \Allfiles\Labs\10\az-500-10_azuredeploy.json 
• \Allfiles\Labs\10\program.cs 
Total Lab Time estimate: 60 minutes 
Exercise 1: Deploy the base infrastructure from an ARM template 
In this exercise, you will complete the following tasks: 
• Task 1: Deploy an Azure VM and an Azure SQL database 
Task 1: Deploy an Azure VM and an Azure SQL database 
In this task, you will deploy an Azure VM, which will automatically install Visual Studio 2019 and SQL 
Server Management Studio 19 as part of the deployment. 
1. Sign-in to the Azure portal https://portal.azure.com/.

<img width="1366" height="768" alt="Screenshot (861)" src="https://github.com/user-attachments/assets/e273c4bd-628e-467d-b565-099500ba9e0e" />

2. In the Azure portal, in the Search resources, services, and docs text box at the top of the Azure portal page, type Deploy a custom template and press the Enter key.

<img width="1366" height="768" alt="Screenshot (869)" src="https://github.com/user-attachments/assets/a394f0de-b0c4-40d4-9e7f-6cbcbf0d1f6c" />

3. On the Custom deployment blade, click the Build your own template in the editor option. 

<img width="1366" height="768" alt="Screenshot (870)" src="https://github.com/user-attachments/assets/9c2003ad-f341-4a41-96ce-fd8196419d28" />

4. On the Edit template blade, click Load file, locate the \Allfiles\Labs\10\az-50010_azuredeploy.json file and click Open.

<img width="1366" height="768" alt="Screenshot (871)" src="https://github.com/user-attachments/assets/3638624c-0d10-4d4b-aab7-99f7f86f54b6" />

5. On the Edit template blade, click Save.

<img width="1366" height="768" alt="Screenshot (872)" src="https://github.com/user-attachments/assets/f08b8d92-0907-4b3f-94e3-f90d871e0150" />

6. On the Custom deployment blade, under Deployment Scope ensure that the following settings are configured (leave any others with their default values):

<img width="1366" height="768" alt="Screenshot (873)" src="https://github.com/user-attachments/assets/6595296d-7411-4cd8-aa19-1ae1c75208fb" />

7. Click the Review and Create button, and confirm the deployment by clicking the Create button. 
This initiates the deployment of the Azure VM and Azure SQL Database required for this lab. 
Do not wait for the ARM template deployment to be completed, but instead continue to the next 
exercise. The deployment might take between 20-25 minutes.

<img width="1366" height="768" alt="Screenshot (880)" src="https://github.com/user-attachments/assets/7b109d7a-6b4b-455b-b9f4-fce05b07d25d" />

Install the az500-10-DB.json Custom Template 
1. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type Deploy a custom template and press the Enter key.

<img width="1366" height="768" alt="Screenshot (869)" src="https://github.com/user-attachments/assets/155438da-be55-4e57-9c15-7c8dcc7047f0" />

2. On the Custom deployment blade, click the Build your own template in the editor option.  

<img width="1366" height="768" alt="Screenshot (870)" src="https://github.com/user-attachments/assets/46f1d6f5-ec6b-4cf8-ac7c-7f9424fadfc1" />

3. On the Edit template blade, click Load file, locate the \Allfiles\Labs\10\az-50010_DB.json file and click Open.

<img width="1366" height="768" alt="Screenshot (876)" src="https://github.com/user-attachments/assets/cbcfef27-9bbc-42b3-96fe-0ef321e4b483" />

4. Ensure the correct Resource Group is selected.

<img width="978" height="518" alt="Screenshot (1155)" src="https://github.com/user-attachments/assets/50831dbe-7dbe-467d-b192-4db0c8e68e22" />

5. Set the Admin Password to the same password you used for the previous step.

<img width="983" height="515" alt="Screenshot (1157)" src="https://github.com/user-attachments/assets/636b36ff-9462-4729-9ebb-5942bd9019e3" />

Exercise 2: Configure the Key Vault resource with a key and a secret 
For all the resources in this lab, we are using the East (US) region. Verify with your instructor this is the 
region to use for your class. 
In this exercise, you will complete the following tasks: 
• Task 1: Create and configure a Key Vault 
• Task 2: Add a key to the Key Vault 
• Task 3: Add a secret to the Key Vault 
Task 1: Create and configure a Key Vault 
In this task, you will create an Azure Key Vault resource. You will also configure the Azure Key Vault 
permissions. 
1. Open the Cloud Shell by clicking the first icon (next to the search bar) at the top right of the 
Azure portal. If prompted, select PowerShell and Create storage.

<img width="978" height="529" alt="Screenshot (1158)" src="https://github.com/user-attachments/assets/2b398269-fe6f-44ed-a47f-f6c0d89826de" />

2. Ensure PowerShell is selected in the drop-down menu in the upper-left corner of the Cloud 
Shell pane.
3. In the PowerShell session within the Cloud Shell pane, run the following to create an Azure 
Key Vault in the resource group AZ500Lab10-lod56968427. (If you chose another name for this 
lab's Resource Group out of Task 1, use that name for this task as well). The Key Vault name 
must be unique. Remember the name you have chosen. You will need it throughout this lab. 
powershellTypeCopy 
$kvName = 'az500kv' + $(Get-Random) 
$location = (Get-AzResourceGroup -ResourceGroupName 'AZ500Lab10
lod56968427').Location 
New-AzKeyVault -VaultName $kvName -ResourceGroupName 'AZ500Lab10-lod56968427' 
Location $location -DisableRbacAuthorization 
The output of the last command will display the vault name and the vault URI. The vault URI is 
in the format https://(vault_name).vault.azure.net/

<img width="990" height="505" alt="Screenshot (1159)" src="https://github.com/user-attachments/assets/ca8db24d-ae5c-4297-be83-1c96a4c19fe5" />

4. Close the Cloud Shell pane.
5. In the Azure portal, in the Search resources, services, and docs text box at the top of the Azure portal page, type Resource groups and press the Enter key.  

<img width="976" height="512" alt="Screenshot (1160)" src="https://github.com/user-attachments/assets/02095d8a-3019-40b2-aa5e-9a9d849076d8" />

6. On the Resource groups blade, in the list of resource group, click the AZ500Lab10lod56968427 (or other name you chose earlier for the resource group) entry.

<img width="977" height="508" alt="Screenshot (1161)" src="https://github.com/user-attachments/assets/4fe8202f-e96d-4378-b4f3-25e1b081b19e" />

7. On the Resource Group blade, click the entry representing the newly created Key Vault.

<img width="979" height="508" alt="Screenshot (1162)" src="https://github.com/user-attachments/assets/eefe227a-683c-44e2-baa3-9ab6fb656d85" />

8. On the Key Vault blade, in the Overview section, click Access Policies and then click + Create.

<img width="966" height="518" alt="Screenshot (1163)" src="https://github.com/user-attachments/assets/e1d63237-e2a2-42aa-9fba-9af347b13439" />

9. On the Create an access policy blade, specify the following settings (leave all others with their default values):

<img width="1366" height="768" alt="Screenshot (905)" src="https://github.com/user-attachments/assets/1405a558-ad81-42a6-b780-a2d1f39dabd0" />


<img width="1366" height="768" alt="Screenshot (905)" src="https://github.com/user-attachments/assets/106de074-504b-4b99-a1d8-bc3731ba2351" />

<img width="1366" height="768" alt="Screenshot (907)" src="https://github.com/user-attachments/assets/b9b83823-fd26-48de-9347-6dea503170d8" />

<img width="1366" height="768" alt="Screenshot (908)" src="https://github.com/user-attachments/assets/afb195ec-ffd4-444d-99bd-1d43ac232477" />

10. The previous Review + create operation returns to the Access policies page that lists Application, Email, Key Permissions, Secret Permissions, and Certificate Permissions.

<img width="1366" height="768" alt="Screenshot (909)" src="https://github.com/user-attachments/assets/57c1ec6f-823b-4f31-93a2-e131e5411bf8" />

Task 2: Add a key to Key Vault 
In this task, you will add a key to the Key Vault and view information about the key. 
1. In the Azure portal, open a PowerShell session in the Cloud Shell pane.

<img width="1366" height="768" alt="Screenshot (910)" src="https://github.com/user-attachments/assets/614ca6f8-0291-4e8e-8ae4-c18c501f6807" />

2. Ensure PowerShell is selected in the upper-left drop-down menu of the Cloud Shell pane.
3. In the PowerShell session within the Cloud Shell pane, run the following to add a software
protected key to the Key Vault: 
powershellTypeCopy 
$kv = Get-AzKeyVault -ResourceGroupName 'AZ500Lab10-lod56968427' 
$key = Add-AZKeyVaultKey -VaultName $kv.VaultName -Name 'MyLabKey' -Destination 
'Software' 
The name of the key is MyLabKey

<img width="1366" height="768" alt="Screenshot (911)" src="https://github.com/user-attachments/assets/ab7a48d3-c007-42f4-a27a-66332386cc69" />

4. In the PowerShell session within the Cloud Shell pane, run the following to verify the key was created: powershellTypeCopy Get-AZKeyVaultKey -VaultName $kv.VaultName

<img width="1366" height="768" alt="Screenshot (912)" src="https://github.com/user-attachments/assets/c66abd39-0e1c-45d3-997f-73adf544c056" />

5. In the PowerShell session within the Cloud Shell pane, run the following to display the key 
identifier: powershellTypeCopy $key.key.kid

<img width="1366" height="768" alt="Screenshot (913)" src="https://github.com/user-attachments/assets/133b0589-f276-45bb-b3e1-1c5e415b3ce2" />

6. Minimize the Cloud Shell pane.
7. Back in the Azure portal, on the Key Vault blade, in the Objects section, click Keys. 

<img width="1366" height="768" alt="Screenshot (914)" src="https://github.com/user-attachments/assets/edab7174-5518-48f8-b0c5-49321a208f03" />

8. In the list of keys, click the MyLabKey entry and then, on the MyLabKey blade, click the 
entry representing the current version of the key. 
Examine the information about the key you created. 
You can reference any key by using the key identifier. To get the most current version, reference 
https://(key_vault_name).vault.azure.net/keys/MyLabKey or get the specific version with: 
https://.vault.azure.net/keys/MyLabKey/(key_version)

<img width="1366" height="768" alt="Screenshot (915)" src="https://github.com/user-attachments/assets/8effd93d-9738-4643-b620-47df0250010f" />

Task 3: Add a Secret to Key Vault
1. Switch back to the Cloud Shell pane
2. In the PowerShell session within the Cloud Shell pane, run the following to create a variable with a secure string value: powershellTypeCopy 
$secretvalue = ConvertTo-SecureString 'Pa55w.rd1234' -AsPlainText -Force

<img width="1366" height="768" alt="Screenshot (916)" src="https://github.com/user-attachments/assets/ab348ea9-1293-4fc3-82fb-26fa76b7d4dd" />

3. In the PowerShell session within the Cloud Shell pane, run the following to add the secret to the vault: powershellTypeCopy 
$secret = Set-AZKeyVaultSecret -VaultName $kv.VaultName -Name 'SQLPassword' SecretValue $secretvalue The name of the secret is SQLPassword.

<img width="1366" height="768" alt="Screenshot (917)" src="https://github.com/user-attachments/assets/83f8d035-d373-4e27-a282-4f411525ca85" />

4. In the PowerShell session within the Cloud Shell pane, run the following to verify the secret was created. powershellTypeCopy 
Get-AZKeyVaultSecret -VaultName $kv.VaultName

<img width="1366" height="768" alt="Screenshot (918)" src="https://github.com/user-attachments/assets/aada6edb-289d-4d8b-a8d7-74cfcec55fd4" />

5. Minimize the Cloud Shell pane.
6. In the Azure portal, navigate back to the Key Vault blade, in the Objects section, click Secrets.

<img width="1366" height="768" alt="Screenshot (919)" src="https://github.com/user-attachments/assets/0ba2dee1-b2f3-4fdf-8a02-ca6eaecce597" />

7. In the list of secrets, click the SQLPassword entry and then, on the SQLPassword blade, click the entry representing the current version of the secret.
Examine the information about the secret you created. 
To get the most current version of a secret, 
reference https://<key_vault_name>.vault.azure.net/secrets/<secret_name> or get a specific 
version, 
reference https://<key_vault_name>.vault.azure.net/secrets/<secret_name>/<secret_version> 

<img width="1366" height="768" alt="Screenshot (920)" src="https://github.com/user-attachments/assets/a92d9fa9-98ff-47fc-b995-490c75af34f1" />

Exercise 3: Configure an Azure SQL database and a data-driven application 
In this exercise, you will complete the following tasks: 
• Task 1: Enable a client application to access the Azure SQL Database service. 
• Task 2: Create a policy allowing the application access to the Key Vault. 
• Task 3: Retrieve SQL Azure database ADO.NET Connection String 
• Task 4: Log on to the Azure VM running Visual Studio 2019 and SQL Management Studio 19 
• Task 5: Create a table in the SQL Database and select data columns for encryption 
Task 1: Enable a client application to access the Azure SQL Database service. 
In this task, you will enable a client application to access the Azure SQL Database service. This will be 
done by setting up the required authentication and acquiring the Application ID and Secret that you will 
need to authenticate your application. 
1. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type App Registrations and press the Enter key.

<img width="1366" height="768" alt="Screenshot (921)" src="https://github.com/user-attachments/assets/e24890aa-bda5-4153-9a44-be002d955387" />

2. On the App Registrations blade, click + New registration.

<img width="1366" height="768" alt="Screenshot (922)" src="https://github.com/user-attachments/assets/83804289-09fe-4ff5-a479-776c8ba3a5da" />

3. On the Register an application blade, specify the following settings (leave all others with their default values):

<img width="1366" height="768" alt="Screenshot (924)" src="https://github.com/user-attachments/assets/98d367e8-2eb7-4730-be3d-fa1be9a431e3" />

<img width="1366" height="768" alt="Screenshot (923)" src="https://github.com/user-attachments/assets/5f7e8933-f008-41bb-85cb-51d7e0829b4c" />

4. On the Register an application blade, click Register. Once the registration is completed, the browser will automatically redirect you to sqlApp blade.

<img width="1366" height="768" alt="Screenshot (925)" src="https://github.com/user-attachments/assets/3fb4a5e9-355c-4dab-a090-c3e852ef0772" />

5. On the sqlApp blade, identify the value of Application (client) ID. Record this value. You will need it in the next task.

<img width="1366" height="768" alt="Screenshot (926)" src="https://github.com/user-attachments/assets/4d8fe614-c7d7-43c3-8437-8f4d6443fef8" />

6. On the sqlApp blade, in the Manage section, click Certificates & secrets.

<img width="1366" height="768" alt="Screenshot (927)" src="https://github.com/user-attachments/assets/fe541b95-d907-4e31-b909-9aa65642afe9" />

7. On the sqlApp | Certificates & secrets blade / Client Secrets section, click + New client secret

<img width="1366" height="768" alt="Screenshot (928)" src="https://github.com/user-attachments/assets/341711e5-df04-449c-bc56-e0dc81643116" />

8. In the Add a client secret pane, specify the following settings:

<img width="1366" height="768" alt="Screenshot (929)" src="https://github.com/user-attachments/assets/38d9983b-e60b-4d4f-b890-1d4e83e6f4fc" />

9. Click Add to update the application credentials.

<img width="1366" height="768" alt="Screenshot (930)" src="https://github.com/user-attachments/assets/e247780d-3a1a-4a1e-bc0f-a3c896e4135c" />

10. On the sqlApp | Certificates & secrets blade, identify the value of Key1. 
Record this value. You will need it in the next task. 
Make sure to copy the value before you navigate away from the blade. Once you do, it is no 
longer possible to retrieve its clear text value. 

<img width="1366" height="768" alt="Screenshot (931)" src="https://github.com/user-attachments/assets/13f526ba-321b-4010-917a-2430c01fa422" />

Task 2: Create a policy allowing the application access to the Key Vault. 
In this task, you will grant the newly registered app permissions to access secrets stored in the Key Vault. 
1. In the Azure portal, open a PowerShell session in the Cloud Shell pane.

<img width="1366" height="768" alt="Screenshot (932)" src="https://github.com/user-attachments/assets/2e0a1db5-b750-4291-8e0b-ca3096bd7d8f" />

2. Ensure PowerShell is selected in the upper-left drop-down menu of the Cloud Shell pane.
3. In the PowerShell session within the Cloud Shell pane, run the following to create a variable 
storing the Application (client) ID you recorded in the previous task (replace the 
(Azure_AD_Application_ID) placeholder with the value of the Application (client) ID): 
powershellTypeCopy 
$applicationId = '<Azure_AD_Application_ID>'

<img width="1366" height="768" alt="Screenshot (933)" src="https://github.com/user-attachments/assets/2d323ff2-a881-43e7-8289-a80e87f39f85" />

4. In the PowerShell session within the Cloud Shell pane, run the following to create a variable 
storing the Key Vault name. 
TypeCopy 
$kvName = (Get-AzKeyVault -ResourceGroupName 'AZ500Lab10-lod56968427').VaultName 
$kvName

<img width="1366" height="768" alt="Screenshot (934)" src="https://github.com/user-attachments/assets/fca23388-dd5a-4760-b014-5fd6be62e588" />

5. In the PowerShell session within the Cloud Shell pane, run the following to grant 
permissions on the Key Vault to the application you registered in the previous task: 
powershellTypeCopy 
Set-AZKeyVaultAccessPolicy -VaultName $kvName -ResourceGroupName AZ500Lab10
lod56968427 -ServicePrincipalName $applicationId -PermissionsToKeys 
get,wrapKey,unwrapKey,sign,verify,list

<img width="1366" height="768" alt="Screenshot (935)" src="https://github.com/user-attachments/assets/6e80ce76-3b17-4fa3-9e36-4ccdac60736b" />

6. Close the Cloud Shell pane.

Task 3: Retrieve SQL Azure database ADO.NET Connection String 
The ARM-template deployment in Exercise 1 provisioned an Azure SQL Server instance and an Azure 
SQL database named medical . You will update the empty database resource with a new table structure 
and select data columns for encryption 
1. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type SQL databases and press the Enter key 

<img width="1366" height="768" alt="Screenshot (937)" src="https://github.com/user-attachments/assets/427c6f5a-795c-4b32-8d35-6584ef69731a" />

2. In the list of SQL databases, click the medical() entry. 
If the database cannot be found, this likely means the deployment you initiated in Exercise 1 has 
not completed yet. You can validate this by browsing to the Azure Resource Group 
"AZ500Lab10-lod56968427" (or the name you chose), and selecting Deployments from the 
Settings pane.

<img width="1366" height="768" alt="Screenshot (939)" src="https://github.com/user-attachments/assets/514da48f-40f1-488b-accf-38168f4b2b84" />

3. On the SQL database blade, in the Settings section, click Connection strings. 
The interface includes connection strings for ADO.NET, JDBC, ODBC, PHP, and Go.
4. Record the ADO.NET (SQL authentication) connection string. You will need it later. 
When you use the connection string, make sure to replace the {your_password} placeholder with 
the password that you configured with the deployment in Exercise 1.

<img width="1366" height="768" alt="Screenshot (940)" src="https://github.com/user-attachments/assets/4a79d1b5-13d3-4b19-9291-e37f554598fc" />

Task 4: Log on to the Azure VM running Visual Studio 2019 and SQL Management Studio 19 
In this task, you log on to the Azure VM, which deployment you initiated in Exercise 1. This Azure VM 
hosts Visual Studio 2019 and SQL Server Management Studio 19. 
Before you proceed with this task, ensure that the deployment you initiated in the first exercise has 
completed successfully. You can validate this by navigating to the blade of the Azure resource group 
"AZ500Lab10-lod56968427" (or other name you chose) and selecting Deployments from the Settings 
pane. 
1. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type virtual machines and press the Enter key.

<img width="1366" height="768" alt="Screenshot (941)" src="https://github.com/user-attachments/assets/8ad90b1a-6137-4fcb-8fc0-79772f8f56c8" />

2. In the list of Virtual Machines shown, select the az500-10-vm1 entry. On the az500-10
vm1 blade, on the Essentials pane, take note of the Public IP address. You will use this later.

<img width="1366" height="768" alt="Screenshot (942)" src="https://github.com/user-attachments/assets/61672fa1-efa7-4e8f-b531-456aad778f13" />

Task 5: Create a table in the SQL Database and select data columns for encryption 
In this task, you will connect to the SQL Database with SQL Server Management Studio and create a 
table. You will then encrypt two data columns using an autogenerated key from the Azure Key Vault. 
1. In the Azure portal, navigate to the blade of the medical SQL database, in 
the Essentials section, identify the Server name (copy to clipboard), and then, in the toolbar, 
click Set server firewall. 
Record the server name. You will need the server name later in this task.

<img width="1366" height="768" alt="Screenshot (945)" src="https://github.com/user-attachments/assets/ec39a0d6-9279-48aa-bcaf-789cc9a2c97c" />

2. On the Firewall settings blade, scroll down to Rule Name, click + Add a firewall rule, and 
specify the following settings: 
Setting 
Rule Name Allow Mgmt VM 
Start IP 
Value 
the Public IP Address of the az500-10-vm1 
End IP 
the Public IP Address of the az500-10-vm1

<img width="1366" height="768" alt="Screenshot (946)" src="https://github.com/user-attachments/assets/0d349968-3c33-438e-b72f-b873148324ae" />

3. Click Save to save the change and close the confirmation pane. 
This modifies the server firewall settings, allowing connections to the medical database from the 
Azure VM's public IP address you deployed in this lab.

<img width="1366" height="768" alt="Screenshot (947)" src="https://github.com/user-attachments/assets/c038c9d1-73da-4931-908b-36d487edeec7" />

4. Navigate back to the az500-10-vm1 blade, click Overview, next click Connect and, in the 
drop down menu, click Connect.

<img width="1366" height="768" alt="Screenshot (948)" src="https://github.com/user-attachments/assets/e83cd5f5-f748-4c6f-8ade-5846bf9aed14" />

5. Download the RDP file and use it to connect to the az500-10-vm1 Azure VM via Remote 
Desktop. When prompted to authenticate, provide the following credentials:

<img width="1366" height="768" alt="Screenshot (950)" src="https://github.com/user-attachments/assets/f5f655c8-d02d-46ee-838b-17da8924b16c" />

6. Wait for the Remote Desktop session and Server Manager to load. Close Server Manager. 
7. The remaining steps in this lab are performed within the Remote Desktop session to the az500
10-vm1 Azure VM.
8. Install https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio
ssms?preserve-view=true&view=sql-server-2017 on az500-10-vm1. Azure VM.

<img width="1366" height="768" alt="Screenshot (951)" src="https://github.com/user-attachments/assets/8e468172-be63-4c70-874a-add9a6d70318" />

9. Open SQL Server Management Studio.

<img width="1366" height="768" alt="Screenshot (952)" src="https://github.com/user-attachments/assets/1efd9f85-1e9f-410f-aeec-e0529629997b" />

10. In the Connect to Server dialog box, specify the following settings:

<img width="1366" height="768" alt="Screenshot (953)" src="https://github.com/user-attachments/assets/2af6a190-c1e0-4b4d-ae80-99c96408522d" />

11. In the Connect to Server dialog box, click Connect.

<img width="1366" height="768" alt="Screenshot (1076)" src="https://github.com/user-attachments/assets/efc66ac3-94be-40fb-b453-8ec5fe21b155" />

12. Within the SQL Server Management Studio console, in the Object Explorer pane, expand the Databases folder.

<img width="1366" height="768" alt="Screenshot (1077)" src="https://github.com/user-attachments/assets/75f83ab9-a0e0-4501-8374-3e4bf41cb0d6" />

13. In the Object Explorer pane, right-click the medical database and click New Query. The medical database can be found in the menu. 

<img width="1366" height="768" alt="Screenshot (1080)" src="https://github.com/user-attachments/assets/80aa9fbf-978b-4b05-ad4d-c354a5a819b0" />

14. 14. Paste the following code into the query window and click Execute. This will create 
a Patients table. 
sqlTypeCopy 
CREATE TABLE [dbo].[Patients]( 
[PatientId] [int] IDENTITY(1,1), 
[SSN] [char](11) NOT NULL, 
[FirstName] [nvarchar](50) NULL, 
[LastName] [nvarchar](50) NULL, 
[MiddleName] [nvarchar](50) NULL, 
[StreetAddress] [nvarchar](50) NULL, 
[City] [nvarchar](50) NULL, 
[ZipCode] [char](5) NULL, 
[State] [char](2) NULL, 
[BirthDate] [date] NOT NULL  
PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] ); 

<img width="1366" height="768" alt="Screenshot (1081)" src="https://github.com/user-attachments/assets/dce2180b-2b48-4b57-a569-ac69822d44c1" />

15. After the table is created successfully, in the Object Explorer pane, expand the medical database node, the tables node, right-click the dbo.Patients node, and click Encrypt 
Columns. This will initiate the Always Encrypted wizard.

<img width="1366" height="768" alt="Screenshot (1082)" src="https://github.com/user-attachments/assets/2250d591-d923-436a-9e97-fb273190da91" />

16. On the Introduction page, click Next.

<img width="1366" height="768" alt="Screenshot (1083)" src="https://github.com/user-attachments/assets/bd3a8c35-6426-4a89-81dc-46e023e903ee" />

17. On the Column Selection page, select the SSN and Birthdate columns, set the Encryption 
Type of the SSN column to Deterministic and of the Birthdate column to Randomized, and 
click Next. 
While performing the encryption if any error thrown like Exception has been thrown by the 
target of an invocation related 
to Rotary(Microsoft.SQLServer.Management.ServiceManagement) then make sure the Key 
Permission's values of Rotation Policy Operations are unchecked, if not in the Azure portal 
navigate to the Key Vault >> Access Policies >> Key Permissions >> Uncheck all the values 
under the Rotation Policy Operations >> Under Privileged Key Operations >> Uncheck Release.

<img width="1366" height="768" alt="Screenshot (1084)" src="https://github.com/user-attachments/assets/e5d1f93d-dad0-4604-9f8f-b1ff4e00a402" />

18. On the Master Key Configuration page, select Azure Key Vault, click Sign in, when prompted, authenticate by using the same user account you used to provision the Azure Key 
Vault instance earlier in this lab, ensure that that Key Vault appears in the Select an Azure Key Vault drop down list, and click Next

<img width="1366" height="768" alt="Screenshot (1104)" src="https://github.com/user-attachments/assets/acb160fa-a029-4a2f-8920-6d6880ba1726" />

19. On the Run Settings page, click Next.

<img width="1366" height="768" alt="Screenshot (1106)" src="https://github.com/user-attachments/assets/9bfb9cb7-7d9b-4e3e-8a13-8941867881f6" />

20. On the Summary page, click Finish to proceed with the encryption. When prompted, sign in again by using the same user account you used to provision the Azure Key Vault instance earlier in this lab.

<img width="1366" height="768" alt="Screenshot (1107)" src="https://github.com/user-attachments/assets/2ac8c51e-cb19-4a1e-bf70-b00f8e4dcea3" />

Exercise 4: Demonstrate the use of Azure Key Vault in encrypting the Azure SQL database 
In this exercise, you will complete the following tasks: 
• Task 1: Install Visual Studio 2022 
• Task 2: Run a data-driven application to demonstrate the use of Azure Key Vault in encrypting the 
Azure SQL database 
Task 1: Install Visual Studio 2022 
1. Switch to your Server virtual machine if you are not already there.
2. Open Server Manager.
3. Select Local Servers.
4. Set IE Enhanced Security Configuration to Off.

<img width="1366" height="768" alt="Screenshot (1110)" src="https://github.com/user-attachments/assets/e4744ff9-2a82-4291-ad9d-4b37ddb22cd6" />

5. Open the Browser and bypass the warning about IE ESC being turned off.
6. Go to https://visualstudio.microsoft.com/downloads.
7. In the Visual Studio 2022 box, under Community select Free download.
8. When the download finishes, select Open File.
9. Select continue to start the install.

<img width="619" height="327" alt="Screenshot (1164)" src="https://github.com/user-attachments/assets/c0a211eb-5ffe-42c5-a352-eeba67c89da4" />

10. Choose .Net Desktop Development in the Workloads screen.

Task 2: Run a data-driven application to demonstrate the use of Azure Key Vault in encrypting the Azure 
SQL database 
You will create a Console application using Visual Studio to load data into the encrypted columns and 
then access that data securely using a connection string that accesses the key in the Key Vault.

1. From the RDP session to the az500-10-vm1, launch Visual Studio 2019 from the Start menu.
2. Switch to the window displaying Visual Studio 2019 welcome message, click the Sign in button and, when prompted, provide the credentials you used to authenticate to the Azure 
subscription you are using in this lab.
3. On the Get started page, click Create a new project.
4. In the list of project templates, search for Console App (.NET Framework), in the list of results, click Console App (.NET Framework) for C#, and click Next.
5. On the Configure your new project page, specify the following settings (leave other settings 
with their default values), then click Create:
6. In the Visual Studio console, click the Tools menu, in the drop down menu, click NuGet Package Manager, and, in the cascading menu, click Package Manager Console.

<img width="625" height="341" alt="Screenshot (1165)" src="https://github.com/user-attachments/assets/1179e043-177d-4cf1-9bde-70681c386c31" />

7. In the Package Manager Console pane, run the following to install the first required NuGet package:
powershellTypeCopy Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
8. In the Package Manager Console pane, run the following to install the second required NuGet package:
powershellTypeCopy Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

<img width="624" height="345" alt="Screenshot (1166)" src="https://github.com/user-attachments/assets/5c3b824b-b67f-4408-95ff-d3e2a0a2fee7" />

9. Minimize the RDP session to your Azure virtual machine, then navigate to \Allfiles\Labs\10\program.cs, open it in Notepad, copy its content into Clipboard. 
These files are located on the Desktop.
10. Return to the RDP session, and in the Visual Studio console, in the Solution Explorer window, click Program.cs and replace its content with the code you copied into 
Clipboard.
11. In the Visual Studio window, in the Program.cs pane, in line 15, replace the (connection string noted earlier) placeholder with the Azure SQL database ADO.NET connection string you 
recorded earlier in the lab. In the connection string, replace the {your_password} placeholder, with the password that you specified in the deployment in Exercise 1. If you saved the string on the lab computer, you may need to leave the RDP session to copy the ADO string, then return to 
the Azure virtual machine to paste it in

<img width="623" height="328" alt="Screenshot (1167)" src="https://github.com/user-attachments/assets/7c9f9985-0b8c-408b-95f1-555e6b140f3b" />

12. In the Visual Studio window, in the Program.cs pane, in line 16, replace the (client id noted earlier) placeholder with the value of Application (client) ID of the registered app you recorded earlier in the lab.
13. In the Visual Studio window, in the Program.cs pane, in line 17, replace the (key value noted earlier) placeholder with the the value of Key1 of the registered app you recorded earlier in the lab.
14. In the Visual Studio console, click the Start button to initiate the build of the console application and start it.

<img width="625" height="332" alt="Screenshot (1168)" src="https://github.com/user-attachments/assets/b3739e88-64a9-417d-a7ff-1a1358b35f14" />

15. The application will start a Command Prompt window. When prompted for password, type the password that you specified in the deployment in Exercise 1 to connect to Azure SQL 
Database.
16. Leave the console app running and switch to the SQL Management Studio console.
17. In the Object Explorer pane, right-click the medical database and, in the right-click menu, click New Query.
18. From the query window, run the following query to verify that the data that loaded into the 
database from the console app is encrypted. sqlTypeCopy SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
19. Switch back to the console application where you are prompted to enter a valid SSN. This 
will query the encrypted column for the data. At the Command Prompt, type the following and 
press the Enter key: cmdTypeCopy 999-99-0003 
Verify that the data returned by the query is not encrypted.

<img width="634" height="364" alt="Screenshot (1169)" src="https://github.com/user-attachments/assets/647865ef-7b73-4f9b-901e-9cc75e9327a9" />

20. To terminate the console app, press the Enter key

# Conclusion
- The implementation of this proof-of-concept successfully validated a critical security architecture 
utilizing Azure SQL Database's Always Encrypted feature seamlessly integrated with Azure Key Vault.
