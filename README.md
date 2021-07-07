## Deploying application configurations, secret references and feature flags to azure application config store.

This repo is a sample reference for managing your application configurations for different environments in Azure app configuration.

### Folder and file structure


    Root
    ├── config
    |   ├──Development-app-config.json      #application configurations for Development environment
    |   ├──Staging-app-config.json          #application configurations for Staging environment
    |   ├──XXXX-app-config.json             #application configurations for other(UAT, Production etc) environments
    ├── featureflags
    |   ├──Development-featureflags.json    #feature flags for Development environment
    |   ├──Staging-featureflags.json        #feature flags for Staging environment
    |   ├──XXXX-featureflags.json           #feature flags for other(UAT, Production etc) environments
    ├── secretrefs
    |   ├──Development-featureflags.json    #secret references for Development environment
    |   ├──Staging-featureflags.json        #secret references for Staging environment  
    |   ├──XXXX-featureflags.json           #secret references for other(UAT, Production etc) environments
    └── pipeline.yaml                       #Yaml pipeline for synching all of these configurations to azure app configuration store.

Notes:-
* Each environment should have its own file in each folder, keep the file with empty json `{}` if there are no values.
* Environment name used in file names is case sensitive.
* Environment name actually represents `Label` in azure app configuration store.

### Pre-Requisites
* Azure Subscription(If you don't have one you can create a [free azure subscription](https://azure.microsoft.com/en-in/free/))
* Azure App Configuration
* KeyVault

### Creating Azure DevOps project and Pipelines
1. Create a new project in Azure DevOps by [importing](https://docs.microsoft.com/en-us/azure/devops/repos/git/import-git-repository?view=azure-devops) this git repository.
2. Create a new [service connection](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml#create-a-service-connection) for your project.
3. Go to your Azure app configuration and under Access Control (IAM) add role assignment with App Configuration Data Owner permissions for the service connection.
4. Update the service connection name in ServiceA/pipeline.yaml and ServiceB/pipeline.yaml files.
5. Install [Azure App Configuration Push Task](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task-push)
6. Create two [deployment pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/clone-import-pipeline?view=azure-devops&tabs=yaml) from ServiceA/pipeline.yaml and ServiceB/pipeline.yaml

 
### Running pipeline
Pipeline accepts two parameters:
* Label(Environment): This represents the name of the environment for which you are deploying the configurations.
* App Configuration Name: This represents the name of the azure app config store where the configurations will be written to.

During execution Label parameter will get mapped to part of the file name in each folder to read the json files for deploying configurations in the given app config store.
All of the configurations will be prefixed with the given prefix, in this example use case Prefix will be ServiceA and ServiceB.

