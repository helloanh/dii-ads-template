# Dii ADS Template with Ruby on Rails Framework

### Requirements   

Tools: [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)    
Rails: 4.2.3
Ruby: 2.3.0

### Installation

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

3. Create resource group. Designate the location that is closest to your geographic location.

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



