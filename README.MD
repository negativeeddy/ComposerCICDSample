This repo is an extension of the sample [Set up continuous integration and continuous delivery for a Composer bot](https://docs.microsoft.com/en-us/composer/how-to-cicd).

This CI/CD pipeline modifies the original in the following ways

1. it has multiple stages. It builds the artifacts then deploys those artifacts into a dev and prod stage without rebuilding the project
2. has an "environmentName" variable which is used in the naming of the LUIS apps. By convention LUIS apps are named "BotName(env)-DialogName.locale.lu"
3. telemetry is enabled via Application Insights
4. bot state is stored in CosmosDb

# Azure Devops Setup

The CI/CD sample has 3 stages: Build, DeployDev, and DeployProd. The build stage builds the web app and the LUIS models. The two deploy stages take the output from the build stage and deploy the app and the LUIS models into their respective Azure resources. 

**NOTE:** The current sample does not build/publish QnA models.

Each stage requires the following variables to be set. It is recommended to have two variable groups for each stage, a regular variable group and a secret group that is loaded from Azure KeyVault.

The sample's groups are named "Secure Bot Dev Variables" and "Secure Bot Dev Secrets" for the dev stage and "Secure Bot Prod Variables" and "Secure Bot Prod Secrets" for the prod stage. If using different names in your pipeline, be sure to update the buildAndDeploy.yaml file with the correct variable group names.

### Main variable group

variable               | name
---------              | ------
azureServiceConnection | name of your service connection to Azure 
botName                | used for LUIS/QnA model names
botProjectDirectory    | the path to the bot's project folder in your repo
botProjectName         | the name of the bot project file (without the file extension)
cosmosDBEndpoint       | e.g. https://contosodb.documents.azure.com:443/
environmentName        | the name of the environment/stage. Used for LUIS/QnA Model names e.g. dev or prod
luisAuthoringRegion    | e.g. west-us
luisEndpoint           | the LUIS prediction endpoint e.g. https://westus.api.cognitive.microsoft.com
microsoftAppId         | the bot's Microsoft App Id
qnaEndpointKey         | not currently used
qnaHostName            | not currently used
qnaKnowledgebaseId     | not currently used
qnaRegion              | not currently used
qnaSubscriptionKey     | not currently used
resourceGroupName      | the name of the Azure Resource Group
webAppName             | name of the Azure App Service

### Secrets Variable Group (stored in keyvault)

secret                       | description
------                       | -----------
LuisAuthoringKey             | LUIS authoring key
LuisEndpointKey              | LUIS prediction key
MicrosoftAppPassword         | The bot's Microsoft App password
appInsightsConnectionString  | The Application Insights connection string
cosmosDbAuthKey              | The CosmosDb key
