# Azure Virtual Machine Actions

[![](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/start-vm.yml/badge.svg)](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/start-vm.yml)
[![](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/stop-vm.yml/badge.svg)](https://github.com/gabrielecanepa/azure-vm-actions/actions/workflows/stop-vm.yml)

This repository contains a collection of GitHub Actions for working with Azure virtual machines.

## Configuration

### Service principal

To create a service principal and configure its access to Azure resources, open a local or cloud shell and run the following command specifying a service principal name, the virtual machine subscription ID and resource group name:

```sh
az ad sp create-for-rbac --name "<SERVICE_PRINCIPAL_NAME>" --role contributor \
    --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME> \
    --sdk-auth
```

The response will be a JSON object with the following structure:

```json
{
  "clientId": "<CLIENT_ID>",
  "clientSecret": "<CLIENT_SECRET>",
  "subscriptionId": "<SUBSCRIPTION_ID>",
  "tenantId": "<TENANT_ID>",
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

- `AZURE_CREDENTIALS`: the JSON object from the previous step
- `VM_NAME`: name of the virtual machine
- `VM_RESOURCE_GROUP`: name of the virtual machine resource group

## Usage
  
Once configured, you can run the following workflows using GitHub Actions.

| Name                                      | Description                                       | Schedule                    | Trigger                         |
| ----------------------------------------- | ------------------------------------------------- | --------------------------- | ------------------------------- |
| [Start VM](.github/workflows/stop-vm.yml) | Start a virtual machine                           | None                        | `workflow_dispatch`             |
| [Stop VM](.github/workflows/stop-vm.yml)  | Stop (power off and deallocate) a virtual machine | Every day at midnight (UTC) | `schedule`, `workflow_dispatch` |

