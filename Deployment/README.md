# Company Communicator App Template - Multi-Tenant Deployment

Below are some considerations for multi-tenant deployment. Multi-tenancy is intended for a split deployment for GCC High and GCC-M + Azure Commercial. Here are the changes made by deployment piece to fit in to the multi-tenant deployment scenario.

### ARM Template
-  There are two ARM templates. One is for the bots in GCC-M + Commercial tenant and the other is for the rest of the infrastructure which wil be in the GCC High tenant

- You can add default values and deploy the templates using the Azure CLI. Here are the commands:
- - az deployment group --resource-group gcc-high-resoruce-group-name --template-file .path\to\file\azuredeploy-GCC-High.json
- - az deployment group --resource-group gcc-m-resoruce-group-name --template-file .\azuredeploy-bots.json

- Removed tenant id function from tenant ID parameter. This must not default to the tenant in which the main infra is deploy and must instead be the tenant ID of the GCC-M + Azure Commercial tenant, where the teams users are homed.

- There are several areas where storage is referenced. The default endpoint for storage account resource ID function is blob.core.windows.net and we must affix ';EndpointSuffix=','core.usgovcloudapi.net' as a segment to all storage resource ID's in order to point to the correct endpoint.

- The senders in the senderUPNlist parameter must be suffixed with the tenent domain of the GCC-M + Azure Commercial tenant, not the GCC High tenant.

- Use either a custom domain name or the default *.azurewebsites.us domain that is listed on the Azure App Service resource. Azure Front Door is not available in GCC High.

### Deployment Guide
- When following the deployment guide, ensure that you follow step 3 for the author app registration and step 4 for the user app registration. Add user.read.all delegated permission for the user app registration

### Known Issues
- The pull from the Git repository takes longer that ARM will wait for completion. Once the template is finished deploying, we noticed that there were issue with NPM in Function Apps. This did not affect the overall use of the application.

- We installed the Company Communicator App to a Teams Channel. We noticed that sometimes the channel that has company communicator users app installed will disappear and reappear. A reload or restart of the Teams App on desktop or web ususally fixes this but may have to be repeated a couple of times.