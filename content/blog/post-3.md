---
title: "Deploy Azure Resources via Terraform and Azure Pipeline"
description: "meta description"
image: "images/post/02/page.png"
date: 2023-04-15T16:56:47+06:00
draft: false
author: "Yuru Zuo"
tags: ["Azure", "DevOps", "Terraform", "Azure Pipeline"]
categories: ["Azure"]
---

# Deploy Azure Resources via Terraform and Azure Pipeline

Azure DevOps and Azure Pipeline are powerful tools for automating the deployment of infrastructure as code. By combining these tools with Terraform, we can deploy Azure resources quickly and efficiently. In this article, we will walk through the steps required to set up Azure DevOps and Azure Pipeline to deploy Azure resources via Terraform.

The first step is to create a service connection in the Azure DevOps project. This connection will allow the pipeline to communicate with Azure and deploy resources.

## **Create Service Connection**

To create a service connection, follow these steps:

1. Navigate to **`Project Settings`** → **`Service connections`**
2. Click on **`New service connection`**
3. Choose **`Azure Resource Manager`**
4. Follow the prompts to create the connection

Once the connection is created, we can move on to creating a variable group. This group will store key-value pairs that can be referenced in the pipeline's YAML file.

## **Create Variable Group**

To create a variable group, follow these steps:

1. Navigate to **`Pipelines`** → **`Library`** → **`Variable group`**
2. Click on **`New variable group`**
3. Give the group a name (e.g., **`dev`**) and add key-value pairs
4. Reference the variable group in the pipeline's YAML file using **`group: dev`** and **`$(SUBSCRIPTION_ID)`**


Finally, we need to add the Terraform tasks to the YAML file. This will allow the pipeline to run Terraform commands and deploy resources.

## **Add Terraform Tasks**

To add Terraform tasks, follow these steps:

1. Install the Terraform extension in Azure DevOps
2. Add Terraform tasks to the pipeline's YAML file

Here's an example of what the YAML file might look like:

```yaml
variables:
  - group: dev
  - name: armServiceConnection
    value: 'ARM_SERVICE_CONNECTION'
- task: AzureCLI@2
        displayName: 'Deploy Web App'
        inputs:
          azureSubscription: ${{ variables.armServiceConnection }}
          scriptType: bash
          scriptLocation: inlineScript
          addSpnToEnvironment: true
          inlineScript: |
            make deploy \
              ARM_CLIENT_ID=$servicePrincipalId \
              ARM_CLIENT_SECRET=$servicePrincipalKey \
              ARM_SUBSCRIPTION_ID=$(SUBSCRIPTION_ID) \
              ARM_TENANT_ID=$tenantId \
              TF_VAR_identifier='$(Build.sourceBranchName)'

```
In this example, the **`AzureCLI`** task is used to log in to Azure and set the subscription. The **`env`** section contains environment variables that Terraform will use to deploy resources.

`azureSubscription` refers to the service connection name that was created earlier.

`ARM_SUBSCRIPTION_ID` is the subscription ID that is stored in the service connection.

`ARM_CLIENT_ID, ARM_CLIENT_SECRET`, and `ARM_TENANT_ID` are other required parameters that are automatically populated once the service connection is created.

`addSpnToEnvironment` needs to be set to true to allow using the service principle for login instead of personal credentials. By default, it is set to false.

Other Terraform environment variables can be set using the `TF_VAR_` prefix.