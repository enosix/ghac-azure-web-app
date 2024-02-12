# Azure Web App Deploy

This GitHub Action is designed to deploy a web app to Azure using the Azure Web Apps Deploy action. It provides a set of
inputs for configuration and executes a series of steps to log into Azure, create a new web app slot if it doesn't exist,
retrieve the publishing profile, update the Azure AD application, deploy the web app, and add a comment to the
pull request.

## Inputs

The action requires several inputs related to Azure and the deployment details:

- `azure_client_id`: The Azure client id. This is required.
- `azure_client_secret`: The Azure client secret. This is required.
- `azure_subscription_id`: The Azure subscription id. This is required.
- `azure_tenant_id`: The Azure tenant id. This is required.
- `azure_webapp_name`: The Azure webapp name. This is required.
- `resource_group`: The resource group. This is required.
- `deployment_slot`: The deployment slot. This is required.
- `image`: The image. Either this or `package` is required.
- `package`: The deployment package. Either this or `image` is required.
- `ad_app_id`: The Azure AD app id. This is optional.

## Steps

The action consists of several steps:

1. **Azure login**: Logs into Azure using the provided credentials.
2. **Clean names**: Cleans the deployment slot name to ensure it only contains alphanumeric characters and hyphens.
3. **Create a new web app slot if it doesn't exist**: Checks if the specified deployment slot exists. If it doesn't, it creates a new one.
4. **Get publish profile**: Retrieves the publishing profile for the web app.
5. **Update AD**: If an Azure AD app id is provided, it updates the redirect URIs of the Azure AD application.
6. **Run Azure webapp deploy action using publish profile credentials**: Deploys the web app using the Azure Web Apps Deploy action.
7. **Add PR comment**: If the action is triggered by a pull request, it adds a comment to the PR with the deployment URL or a failure message.

## Usage

To use this action, include it in your workflow file with the required inputs. Here's an example:

```yaml
- name: Docker Push to Github
  uses: enosix/ghac-azure-web-app@stable
  with:
    azure_client_id: ${{ secrets.AZURE_CLIENT_ID }}
    azure_client_secret: ${{ secrets.AZURE_CLIENT_SECRET }}
    azure_subscription_id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
    azure_tenant_id: ${{ secrets.AZURE_TENANT_ID }}
    azure_webapp_name: 'your-webapp-name'
    resource_group: 'your-resource-group'
    deployment_slot: 'your-deployment-slot'
    package: 'path/to/your/package'
    ad_app_id: 'your-ad-app-id'
```

In this example, replace `'your-webapp-name'`, `'your-resource-group'`, `'your-deployment-slot'`, `'path/to/your/package'`,
and `'your-ad-app-id'` with your actual web app name, resource group, deployment slot, image, package, 
and Azure AD app id respectively. Also, make sure to store your Azure client id, client secret, subscription id, and 
tenant id as secrets in your GitHub repository.