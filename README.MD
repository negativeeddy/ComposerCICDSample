This repo is a sample bot which is
 1. built with composer
 2. deployed via Azure Devops Pipeline
    - builds and deploys LUIS (Qna currently disabled)
    - builds and deploys the web app
    - secrets are stored in Azure Key Vault
    - has an "environmentName" variable which is used in the naming of the LUIS apps. Composer names LUIS apps with the convention "BotName(env)-DialogName.locale.lu"
 3. <TODO> has secure networking set up around it in Azure