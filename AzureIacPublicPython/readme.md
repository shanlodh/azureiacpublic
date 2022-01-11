### How this folder works

Azure Python SDK requires the *create-for-rbac* command and the *DefaultAzureCredential* class: 

- see [here](https://docs.microsoft.com/en-us/azure/developer/python/configure-local-development-environment?tabs=cmd#what-the-create-for-rbac-command-does) for the *create-for-rbac* command and use it to save AZURE_SUBSCRIPTION_ID, AZURE_TENANT_ID, AZURE_CLIENT_ID, AZURE_CLIENT_SECRET variables in a secrets file that can be set as environment variables at run time 
- read the docs for the [DefaultAzureCredential](https://docs.microsoft.com/en-us/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) class that handles environment specific identities (Authentication) that request the relevant access tokens (Authorization) 

Use the [rg from env](20210417_AzRgFromEnvPython.py) script to: 

- create DefaultAzureCredential instance
- create ResourceManagementClient instance using the DefaultAzureCredential instance
- use the ResourceManagementClient instance to check if the resource group already exists or else create it 

Use the [storage account](20210418_AzStorageAccountPython.py) script to: 

- create DefaultAzureCredential instance
- create StorageManagementClient instance using the DefaultAzureCredential instance  
- use the StorageManagementClient instance to check if the storage account already exists or else create it 
- use the StorageManagementClient instance to retrieve the account's primary access key and generate a connection string
- use the StorageManagementClient instance to provision a blob container in the account

Use the [csv to blob](https://github.com/shanlodh/azureiacpy/blob/main/AzurePython/20210607_CsvToAzBlobPython.py) script to: 

- create DefaultAzureCredential instance
- create StorageManagementClient instance using the DefaultAzureCredential instance

Please contact the repo author if you wish to have further access to the underlying code 



