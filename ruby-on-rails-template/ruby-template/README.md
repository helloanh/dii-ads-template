# Dii ADS Template with Ruby on Rails Framework  

### Requirements   

Tools: [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)    
Rails: 4.2.3
Ruby: 2.3.0

### Azure Configuration 

1. Login 

``` bash
az login
```

Output

```bash
[
  {
    "cloudName": "AzureCloud",
    "id": "ab9d3d3e-8de4-4b26-baa4-c2ffd5847ef8",
    "isDefault": true,
    "name": "Free Trial",
    "state": "Enabled",
    "tenantId": "d5fe813e-0caa-432a-b2ac-d555aa91bd1c",
    "user": {
      "name": "YOUR_ACCOUNT_USER_NAME",
      "type": "user"
    }
  }
]
```

YOUR_ACCOUNT_USER_NAME is your VA account login credentials to the Azure environment.  

2. Configure a deployment user. The developer in the project team might provide you with the deployment 
user name and password, else you could set a new username and password.  

```bash
az webapp deployment user set --user-name <MY_USER_NAME> --password <MY_PASSWORD>
``` 

Output

```bash
{
  "id": null,
  "kind": null,
  "name": "web",
  "publishingPassword": null,
  "publishingPasswordHash": null,
  "publishingPasswordHashSalt": null,
  "publishingUserName": "MY_USER_NAME",
  "scmUri": null,
  "type": "Microsoft.Web/publishingUsers/web"
}
```

3. Create resource group. Designate the location that is closest to your geographic location.  Replace **myDiiRubyRG** with your own resource group.  

```bash
# list all location
az account list-locations

# create resource group with your selected location
az group create --name myDiiRubyRG --location "East US"
```
Output 

```bash
{
  "id": "/subscriptions/ab9d3d3e-8de4-4b26-baa4-c2ffd5847ef8/resourceGroups/myDiiRubyRG",
  "location": "eastus",
  "managedBy": null,
  "name": "myDiiRubyRG",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

4. Create Azure App Service Plan

```bash
az appservice plan create --name myRailsServicePlan --resource-group myDiiRubyRG  --sku B1 --is-linux

```

### Rails Web App  

1. Create a web app in service plan called **myRailsServicePlan** you created earlier in Azure Configuration, step 4.    


In bash shell for MacOS or Linux OS  

 ```bash 
az webapp create --resource-group myDiiRubyRG --plan myRailsServicePlan --name <app-name> --runtime "RUBY|2.6.2" --deployment-local-git
 ```

In powershell for Windows OS

 ```powershell
az --% webapp create --resource-group myDiiRubyRG --plan myRailsServicePlan --name <app-name> --runtime "RUBY|2.6.2" --deployment-local-git
```

You should receive a JSON output to verify the app is created successfully
```bash
Local git is configured with url of 'https://604307@ruby-template-azure.scm.azurewebsites.net/ruby-template-azure.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "ruby-template-azure.azurewebsites.net",
  "deploymentLocalGitUrl": "https://604307@ruby-template-azure.scm.azurewebsites.net/ruby-template-azure.git",
  "enabled": true,
  "enabledHostNames": [
    "ruby-template-azure.azurewebsites.net",
    "ruby-template-azure.scm.azurewebsites.net"
  ],
  "ftpPublishingUrl": "ftp://waws-prod-blu-077.ftp.azurewebsites.windows.net/site/wwwroot",
  ...
  < JSON data removed for brevity. >
}
```


Copy the value for "defaultHostName," open your web browser, and paste the url value to navigate to your Ruby default app. You should see the following default splash page:  

![default-azure-rails](../screenshots/azure-default-screenshot.jpg)

