## About AzureIacPy

This repo shows how to run **Azure** using code (**Iac**) with the Python (**Py**) SDK. 

Install PowerShell Az module:  

   `Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force`
   
   `Set-ExecutionPolicy RemoteSigned`
   
Use PowerShell to:

   * connect to Azure Active Directory (AD)    

   ```powershell
   $secretsjson = Get-Content ".\secrets\azuresecrets.json" | ConvertFrom-Json
   $AzLogin = $secretsjson.azlogin
   $AzPassword = $secretsjson.azpassword
   $AzPasswordSecure = ConvertTo-SecureString $AzPassword -AsPlainText -Force
   $AzCredentials = New-Object System.Management.Automation.PSCredential ($AzLogin, $AzPasswordSecure)
   Connect-AzAccount -Credential $AzCredentials
```
   * alternatively if you wish to type in login and password use the following  

   `Connect Az-Account`
   
   * after AD login change any Azure context variables as required, e.g
   
   ` Set-AzContext -Subscription $desiredSubscriptionName`
   
   * assign relevant Azure contexts to local variables  
   
   ```powershell

   
   if ("AZURE_TENANT_ID" -in $secretsjson.PsObject.Properties.Name) 
	{
		$secretsjson.AZURE_TENANT_ID = (Get-AzContext).Tenant.Id
	} 
	else 
	{
		$secretsjson | Add-Member -Type NoteProperty -Name 'AZURE_TENANT_ID' -Value (Get-AzContext).Tenant.Id
	}
	
```
   ```powershell
	
	if ("AZURE_SUBSCRIPTION_ID" -in $secretsjson.PsObject.Properties.Name) 
	{
		$secretsjson.AZURE_SUBSCRIPTION_ID = (Get-AzContext).Subscription.Id
	} 
	else 
	{
		$secretsjson | Add-Member -Type NoteProperty -Name 'AZURE_SUBSCRIPTION_ID' -Value (Get-AzContext).Subscription.Id
	}
```

      
   * create ServicePrincipal for using Azure assets  

   ` $sp = New-AzADServicePrincipal -DisplayName $spname`

   * save Id and Secret of the ServicePrincipal  

   ```powershell
	
	if ("AZURE_CLIENT_ID" -in $secretsjson.PsObject.Properties.Name) 
	{
		$secretsjson.AZURE_CLIENT_ID = $sp.id
	} 
	else 
	{
		$secretsjson | Add-Member -Type NoteProperty -Name 'AZURE_CLIENT_ID' -Value $sp.id
	}
```
   ```powershell
	$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($sp.Secret)
	$sp.secret = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)`
	
	if ("AZURE_CLIENT_SECRET" -in $secretsjson.PsObject.Properties.Name) 
	{
		$secretsjson.AZURE_CLIENT_SECRET = $sp.secret
	} 
	else 
	{
		$secretsjson | Add-Member -Type NoteProperty -Name 'AZURE_CLIENT_SECRET' -Value $sp.secret
	}
```
   `$secretsjson | ConvertTo-Json | Set-Content ".\secrets\azuresecrets.json"`
   
Alternate method using `az cli` to generate above credentials is outlined in the [AzurePython readme](https://github.com/shanlodh/azureiacpy/blob/main/AzurePython/readme.md)

Once this setup is complete we can start using Python as shown in the [AzurePython directory](https://github.com/shanlodh/azureiacpy/blob/main/AzurePython) 
   
   
   
   
   