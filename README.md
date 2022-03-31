# Azure DevOps Yaml Templates

These templates are to be included in other build and deploy yaml pipelines to promote reuse of code.

| Name                  | Description                                                                    |
| --------------------- | ------------------------------------------------------------------------------ |
| build-push-docker.yml | Build a Docker image and if on `development`,`master`, or `production` push it |
| dump-environment.yml  | Dump out the build environment, folder, etc.                                   |
| post-slack-step.yml   | Post success or failure messages to Slack                                      |
| upgrade-app-helm.yaml | Get an Azure Vault, do substitutions and do a helm upgrade using the app chart|

## Using This Repo

First, in your main yaml, include this repo as a resource

```yaml
resources:
  repositories:
  - repository: templates
    type: github
    name: jimAtLoyal/devops-templates
    ref: releases/v1.0
    endpoint: loyalhealth
```

Then to refer to any of the templates by its path followed by `@templates`. Here are a couple examples.

```yaml
steps:
    - template: /templates/post-slack-step.yml@templates
      parameters:
        successMessage: 'VSTS: Loyal Core $(Build.SourceBranchName) - Build and nuget push success'
        failureMessage: 'VSTS: Loyal Core $(Build.SourceBranchName) - Build and nuget push failed'

```

```yaml
steps:
    - template: templates/upgrade-app-helm.yml@templates
      parameters:
        rootDirectory: 'Loyal.Kafka/DevOps/helm'
        releaseName: 'kafka-test-consumer'
        valueFile: 'values-test.yaml'
        vaultAzureSubscription: azurekeyvault-Loyal Kafka
        azureSubscription: ${{ variables.azSubscription }}
        dryRun: ${{ parameters.dryRun }}
        inlineVariables: 'testApp: consumer'
```

## Links

Microsoft Doc

- [YAML schema reference for Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#template-references)
- [Templates - Azure Pipelines | Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops)
- [Expressions - Azure Pipelines | Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/expressions?view=azure-devops)
