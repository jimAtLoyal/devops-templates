# Azure DevOps Yaml Templates

These templates are to be included in other build and deploy yaml pipelines to promote reuse of code.

| Name                  | Description                                                                    |
| --------------------- | ------------------------------------------------------------------------------ |
| build-push-docker.yml | Build a Docker image and if on `development`,`master`, or `production` push it |
| dump-environment.yml  | Dump out the build environment, folder, etc.                                   |
| post-slack-step.yml   | Post success or failure messages to Slack                                      |

## Using This Repo

First in your main yaml, include the templates repo as a resource

```yaml
resources:
  repositories:
  - repository: templates
    type: github
    name: jimAtLoyal/devops-templates
    ref: releases/v1.0
```

Then to refer to any of the templates, refer to it by path followed by `@templates`

```yaml
    - template: /templates/post-slack-step.yml@templates
      parameters:
        successMessage: 'VSTS: Loyal Core $(Build.SourceBranchName) - Build and nuget push success'
        failureMessage: 'VSTS: Loyal Core $(Build.SourceBranchName) - Build and nuget push failed'

```

## Links

Microsoft Doc

[YAML schema reference for Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#template-references)
[Templates - Azure Pipelines | Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops)
[Expressions - Azure Pipelines | Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/expressions?view=azure-devops)
