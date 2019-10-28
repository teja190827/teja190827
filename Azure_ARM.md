## Create and deploy Azure Resource Manager templates by using the Azure portal

1.	Sign in to the Azure portal.
2.	Select Create a resource > Storage > Storage account - blob, file, table, queue.
3.	Enter the following information:

![](azure1.png)

![](azure2.png)

4.	Select Review + create on the bottom of the screen. Do not select Create in the next step.
5.	Select Download a template for automation on the bottom of the screen. The portal shows the generated template:

![](azure3.png)

6.	Select Download from the top of the screen.
7.	Open the downloaded zip file, and then save template.json to your computer. In the next section, you use a template deployment tool to edit the template.
8.	Select the Parameter tab to see the values you provided for the parameters. Write down these values, you need them in the next section when you deploy the template.


![](azure4.png)

Using both the template file and the parameters file, you can create a resource, in this tutorial, an Azure storage account.

Edit and deploy the template
The Azure portal can be used to perform some basic template editing

Azure requires that each Azure service has a unique name. The deployment could fail if you entered a storage account name that already exists. To avoid this issue, you modify the template to use a template function call uniquestring() to generate a unique storage account name.

In the Azure portal, select Create a resource.
In Search the Marketplace, type template deployment, and then press ENTER.
Select Template deployment.

![](azure5.png)

1.	Select Create.
2.	Select Build your own template in the editor.
3.	Select Load file, and then follow the instructions to load template.json you downloaded in the last section.
4.	Make the following three changes to the template:

![](azure6.png)

•	Remove the storageAccountName parameter as shown in the previous screenshot.
•	Add one variable called storageAccountName as shown in the previous screenshot:
JSONCopy
"storageAccountName": "[concat(uniqueString(subscription().subscriptionId), 'storage')]"
Two template functions are used here: concat() and uniqueString().
•	Update the name element of the Microsoft.Storage/storageAccounts resource to use the newly defined variable instead of the parameter:

```
"name": "[variables('storageAccountName')]"
```

The final template shall look like:
```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "location": {
          "type": "string"
      },
      "accountType": {
          "type": "string"
      },
      "kind": {
          "type": "string"
      },
      "accessTier": {
          "type": "string"
      },
      "supportsHttpsTrafficOnly": {
          "type": "bool"
      }
  },
  "variables": {
      "storageAccountName": "[concat(uniqueString(subscription().subscriptionId), 'storage')]"
  },
  "resources": [
      {
          "name": "[variables('storageAccountName')]",
          "type": "Microsoft.Storage/storageAccounts",
          "apiVersion": "2018-07-01",
          "location": "[parameters('location')]",
          "properties": {
              "accessTier": "[parameters('accessTier')]",
              "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
          },
          "dependsOn": [],
          "sku": {
              "name": "[parameters('accountType')]"
          },
          "kind": "[parameters('kind')]"
      }
  ],
  "outputs": {}
}

```

1.	Select Save.
2.	Enter the following values:

```
Resource group -> Select the resource group name you created in the last section.
Location -> Select a location for the storage account. For example, Central US.
Account Type -> Enter Standard_LRS for this quickstart.
Kind -> Enter StorageV2 for this quickstart.
Access Tier -> Enter Hot for this quickstart.
Https Traffic Only Enabled -> Select true for this quickstart.
I agree to the terms and conditions stated above -> (select)
```
![](azure7.png)

Here is a screenshot of a sample deployment:

![](azure8.png)

1.	Select Purchase.
2.	Select the bell icon (notifications) from the top of the screen to see the deployment status. You shall see Deployment in progress. Wait until the deployment is completed.

Select Go to resource group from the notification pane. You shall see a screen similar to:

![](azure9.png)
You can see the deployment status was successful
