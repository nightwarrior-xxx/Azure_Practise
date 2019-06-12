## Hosting a PHP webapp on Azure

- Set User for webapp deployment

```
az webapp deployment user set --user-name <name> --password <password>
```

- Create a resource group 

```
az group create --resource-group myResourceGroup --location eastus
```

-  Create an Azure AppService Plan

```
az appservice plan create --resource-group myResourceGroup --name myAppServicePlan --sku FREE
```
where ```sku``` stands for stock keep unit.

- Create webapp

```
az webapp create --plan myAppServicePlan --resource-group myResourceGroup --name tutoverflow --runtime "PHP|7.0" --deployment-local-git
```

