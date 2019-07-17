# Dii ADS Template with Ruby on Rails Framework  

## Azure Requirements   

Tools: [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)      
Rails: 4.2.3  
Ruby: 2.3.0


It is possible to run the following commands on the Azure Cloud Shell in the browser, however Azure CLI is recommended.  


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
    "id": "000060c0-dae5-4fe3-86c8-aeefc4a51e76",
    "isDefault": true,
    "name": "Free Trial",
    "state": "Enabled",
    "tenantId": "00000006-0000-0000-0000-7bd2ccff0000",
    "user": {
      "name": "first.last@va.gov",
      "type": "user"
    }
  }
]
```

The first.last@va.gov is your VA account login credentials to the Azure environment.  

2. Configure a deployment user. The developer in the project team might provide you with the deployment  user name and password, else you could set a new username and password.  User name must be globally unique name. 

```bash
# the format
az webapp deployment user set --user-name <MY_USER_NAME> --password <MY_PASSWORD>

# my example
az webapp deployment user set --user-name dii-rails-user-1 —password <secretpwhere>
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
  "publishingUserName": "dii-rails-user-1",
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
  "id": "/subscriptions/43de60c0-0000-0000-0000-aeefc4a51e76/resourceGroups/myDiiRubyRG",
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

Output

```bash

{
  "freeOfferExpirationTime": "2019-08-16T01:39:38.820000",
  "geoRegion": "East US",
  "hostingEnvironmentProfile": null,
  "hyperV": false,
  "id": "/subscriptions/43de60c0-dae5-4fe3-86c8-aeefc4a51e76/resourceGroups/myDiiRubyRG/providers/Microsoft.Web/serverfarms/myRailsServicePlan",
  "isSpot": false,
  "isXenon": false,
  "kind": "linux",
  "location": "East US",
  "maximumElasticWorkerCount": 1,
  "maximumNumberOfWorkers": 3,
  "name": "myRailsServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  ...

  < JSON data removed for brevity. >
}
```

### Rails Web App  

The steps below act as a template on how to configure your project Rails application as a published github repository to the Azure cloud service. 


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
Local git is configured with url of 'https://dii-rails-user-1@dii-rails-template.scm.azurewebsites.net/dii-rails-template.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "dii-rails-template.azurewebsites.net",
  "deploymentLocalGitUrl": "https://dii-rails-user-1@dii-rails-template.scm.azurewebsites.net/dii-rails-template.git",
  "enabled": true,
  "enabledHostNames": [
    "dii-rails-template.azurewebsites.net",
    "dii-rails-template.scm.azurewebsites.net"
  ],
  "ftpPublishingUrl": "ftp://waws-prod-blu-137.ftp.azurewebsites.windows.net/site/wwwroot",
  "geoDistributions": null,

  ...

  < JSON data removed for brevity. >
}
```

Keep the local git configured url from the json export.  For this example, it is *https://dii-rails-user-1@dii-rails-template.scm.azurewebsites.net/dii-rails-template.git*.  This is the url you will use for step 2 in deployment.  

Copy the value for "defaultHostName," open your web browser, and paste the url value to navigate to your Ruby default app. You should see the following default splash page:  

![default-azure-rails](azure-default-screenshot.jpg)

2. Deploy application   

Change into the local directory of your rails application.  Then remote add the application to azure, then push to azure master to deploy.  

```bash
#format 
git remote add azure <Your git genetated deployment URL>
git push azure master

# my example
git remote add azure ttps://604307@ruby-template-azure.scm.azurewebsites.net/ruby-template-azure.git
git push azure master
```

You will need to input your password that you created.    

Deployment might take a few minutes, so grab a cup of coffee and come back.  

When the process finished, you should see the following lines.  

```bash
...

remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://dii-rails-template.scm.azurewebsites.net/dii-rails-template.git
 * [new branch]      master -> master

```

3. Sanity check in the browser for changes.  

Go back to the browser to see your application updated from default Azure Rails app to your own Rails application.






