# Azure Virtual Machine Actions

[![](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/start-vm.yml/badge.svg)](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/start-vm.yml)
[![](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/stop-vm.yml/badge.svg)](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/stop-vm.yml)

This repository contains a collection of GitHub Actions for working with Azure Virtual Machines.

## Authentication

### Service principal

When you select a virtual machine in the Azure portal, you will see a similar URL in the browser’s address bar:

```
https://portal.azure.com/#@domain.com/resource/subscriptions/
<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Compute/
virtualMachines/<VM_NAME>/overview
```

From this URL, extract the subscription ID, resource group and VM name.

To get the full credentials, open a cloud shell in the Azure portal and run the following command:

```sh
az ad sp create-for-rbac --name "some-name-here" --role contributor \
    --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME> \
    --sdk-auth
```

The response will be a JSON object with the following structure:

```json
{
  "clientId": "CLIENT_ID",
  "clientSecret": "CLIENT_SECRET",
  "subscriptionId": "SUBSCRIPTION_ID",
  "tenantId": "TENANT_ID",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

### GitHub Actions

Add the following secrets to your GitHub repository:

- `VM_CREDENTIALS`: the JSON object from the previous step
- `VM_NAME`: name of the virtual machine taken from the URL
- `VM_RESOURCE_GROUP`: name of the resource group taken from the URL

## Usage
  
Once configured, you can run the following workflows using GitHub Actions.

| Name | Description | Schedule | Trigger |
| --- | --- | --- | --- |
| [Start virtual machine](.github/workflows/stop-vm.yml) | Start a virtual machine | None | `workflow_dispatch` |
| [Stop virtual machine](.github/workflows/stop-vm.yml) | Stop (power off and deallocate) a virtual machine | Every day at midnight | `schedule`, `workflow_dispatch` |

