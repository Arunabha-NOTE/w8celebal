# Azure DevOps and CI/CD Configurations

## Table of Contents
* [Configure Dashboard and Queries for Work Items](#configure-dashboard-and-queries-for-work-items)
* [Use Pipeline Variables While Configuring Pipelines](#use-pipeline-variables-while-configuring-pipelines)
* [Use Variable and Task Groups in Pipelines and Set Scopes for Different Stages](#use-variable-and-task-groups-in-pipelines-and-set-scopes-for-different-stages)
* [Create a Service Connection](#create-a-service-connection)
* [Create a Linux/Windows Self-Hosted Agent](#create-a-linuxwindows-self-hosted-agent)
* [Apply Pre and Post-Deployment Approvers in the Release Pipeline](#apply-pre-and-post-deployment-approvers-in-the-release-pipeline)
* [Create a CI/CD Pipeline to Build and Push a Docker Image to ACR and Deploy to AKS](#create-a-cicd-pipeline-to-build-and-push-a-docker-image-to-acr-and-deploy-to-aks)
* [Create a CI/CD Pipeline to Build and Push a Docker Image to ACR and Deploy to ACI](#create-a-cicd-pipeline-to-build-and-push-a-docker-image-to-acr-and-deploy-to-aci)
* [Create a CI/CD Pipeline to Build a .NET Application and Deploy it to Azure App Service](#create-a-cicd-pipeline-to-build-a-net-application-and-deploy-it-to-azure-app-service)
* [Create a CI/CD Pipeline to Build a React Application and Deploy it to an Azure Virtual Machine](#create-a-cicd-pipeline-to-build-a-react-application-and-deploy-it-to-an-azure-virtual-machine)

---

### Configure Dashboard and Queries for Work Items

1.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization in the Azure portal.
2.  **Select Project:** Choose the project you want to configure.
3.  **Go to Boards > Queries:** In the left-hand navigation, select "Boards" then "Queries".
4.  **Create New Query:** Click "New query" to define your work item query.
5.  **Define Query Criteria:** Add clauses to filter work items (e.g., by assigned to, state, work item type).
6.  **Save Query:** Save the query (e.g., "My Open Bugs").
7.  **Go to Dashboards:** In the left-hand navigation, select "Dashboards".
8.  **Add New Dashboard:** Click "New Dashboard" or select an existing one.
9.  **Add Widget:** Click "Add a widget".
10. **Select Query Tile/Chart:** Choose a widget like "Query Tile" or "Chart for work items" and configure it to use your saved query.
11. **Save Dashboard:** Save your dashboard changes.

### Use Pipeline Variables While Configuring Pipelines

1.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
2.  **Select Project:** Choose your project.
3.  **Go to Pipelines > Pipelines:** In the left-hand navigation, select "Pipelines" then "Pipelines".
4.  **Edit Pipeline:** Select an existing pipeline or create a new one.
5.  **Go to Variables Tab:** In the pipeline editor, go to the "Variables" tab.
6.  **Add New Variable:** Click "+ Add" to add a new variable (e.g., `WebAppName`).
7.  **Set Value:** Provide a value for the variable.
8.  **Use in Tasks:** In your pipeline tasks, reference the variable using `$(VariableName)` (e.g., `$(WebAppName)`).

### Use Variable and Task Groups in Pipelines and Set Scopes for Different Stages

1.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
2.  **Select Project:** Choose your project.
3.  **Create Variable Group:**
    * Go to "Pipelines > Library".
    * Click "+ Variable group".
    * Give it a name (e.g., `DevEnvironmentVars`).
    * Add variables (e.g., `ConnectionString`).
    * Save the group.
4.  **Create Task Group:**
    * Go to "Pipelines > Task groups".
    * Click "+ New task group".
    * Add tasks to the group (e.g., "Azure CLI" task).
    * Save the task group.
5.  **Use in Pipeline:**
    * Edit your pipeline.
    * **Link Variable Group:** In the "Variables" tab, click "Variable groups" and link your created group.
    * **Add Task Group:** In a job, search for your task group and add it.
    * **Set Scope (YAML):** For stage-specific variables, define them within the stage definition in a YAML pipeline:
        ```yaml
        stages:
        - stage: Dev
          variables:
            Environment: 'Dev'
          jobs:
          - job: Build
            steps:
            - script: echo $(Environment)
        - stage: Prod
          variables:
            Environment: 'Prod'
          jobs:
          - job: Deploy
            steps:
            - script: echo $(Environment)
        ```
    * **Set Scope (Classic Editor):** In a classic release pipeline, you can define variables at the release, stage, or job level.

### Create a Service Connection

1.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
2.  **Select Project:** Choose your project.
3.  **Go to Project settings:** In the bottom-left, click "Project settings".
4.  **Select Service connections:** Under "Pipelines", click "Service connections".
5.  **Create New Service Connection:** Click "New service connection".
6.  **Select Service Type:** Choose the type (e.g., "Azure Resource Manager").
7.  **Authentication Method:** Select an authentication method (e.g., "Service principal (automatic)" or "Service principal (manual)").
8.  **Configure Details:** Follow the prompts to configure the connection details (e.g., Azure subscription, resource group).
9.  **Grant Permissions:** Ensure the service connection has the necessary permissions to the Azure resources.
10. **Verify and Save:** Verify the connection and save it.

### Create a Linux/Windows Self-Hosted Agent

1.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
2.  **Select Project:** Choose your project.
3.  **Go to Project settings:** In the bottom-left, click "Project settings".
4.  **Select Agent pools:** Under "Pipelines", click "Agent pools".
5.  **Add Pool:** Click "Add pool".
6.  **Select Type:** Choose "Self-hosted".
7.  **Name Pool:** Give the pool a name (e.g., `MySelfHostedPool`).
8.  **Create Pool:** Click "Create".
9.  **Download Agent:** Click on the created pool, then click "New agent". Follow the instructions to download the agent software for Linux or Windows.
10. **Install and Configure:** Run the downloaded script to configure the agent on your desired machine. This involves specifying the Azure DevOps URL, personal access token (PAT), and the agent pool name.
11. **Start Agent:** Start the agent service.

### Apply Pre and Post-Deployment Approvers in the Release Pipeline

1.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
2.  **Select Project:** Choose your project.
3.  **Go to Pipelines > Releases:** In the left-hand navigation, select "Pipelines" then "Releases".
4.  **Edit Release Pipeline:** Select an existing release pipeline or create a new one.
5.  **Pre-deployment Approvals:**
    * Hover over the stage where you want to add approvals.
    * Click the "Pre-deployment conditions" icon (usually a lightning bolt or person icon).
    * Enable "Pre-deployment approvals".
    * Add users or groups as approvers.
    * Configure policies (e.g., "All approvers must approve").
6.  **Post-deployment Approvals:**
    * Hover over the stage where you want to add approvals.
    * Click the "Post-deployment conditions" icon.
    * Enable "Post-deployment approvals".
    * Add users or groups as approvers.
    * Configure policies.
7.  **Save Pipeline:** Save your release pipeline changes.

### Create a CI/CD Pipeline to Build and Push a Docker Image to ACR and Deploy to AKS

1.  **Prerequisites:** Azure Container Registry (ACR) and Azure Kubernetes Service (AKS) must be set up in your Azure subscription.
2.  **Service Connection:** Ensure you have a service connection to your Azure subscription (as described above).
3.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
4.  **Select Project:** Choose your project.
5.  **Go to Pipelines > Pipelines:** In the left-hand navigation, select "Pipelines" then "Pipelines".
6.  **New Pipeline:** Click "New pipeline".
7.  **Connect to Repo:** Select your code repository (e.g., Azure Repos Git, GitHub).
8.  **Configure Pipeline:**
    * **YAML (Recommended):** Use a YAML file (`azure-pipelines.yml`) in your repository.
        ```yaml
        trigger:
        - main

        variables:
          dockerRegistryServiceConnection: 'YourACRServiceConnectionName' # Name of your service connection to ACR
          imageRepository: 'your-app-image'
          dockerfilePath: '$(Build.SourcesDirectory)/Dockerfile'
          tag: '$(Build.BuildId)'
          aksServiceConnection: 'YourAKSServiceConnectionName' # Name of your service connection to AKS
          kubernetesNamespace: 'default'
          kubernetesSecret: 'your-acr-secret' # Kubernetes secret for ACR pull
          aksClusterName: 'your-aks-cluster'

        stages:
        - stage: Build
          displayName: Build and Push Docker Image
          jobs:
          - job: BuildAndPush
            displayName: Build and Push
            pool:
              vmImage: 'ubuntu-latest'
            steps:
            - task: Docker@2
              displayName: Build and push an image to ACR
              inputs:
                containerRegistry: $(dockerRegistryServiceConnection)
                repository: $(imageRepository)
                command: 'buildAndPush'
                Dockerfile: $(dockerfilePath)
                tags: |
                  $(tag)

        - stage: Deploy
          displayName: Deploy to AKS
          jobs:
          - job: DeployToAKS
            displayName: Deploy to AKS
            pool:
              vmImage: 'ubuntu-latest'
            steps:
            - task: KubernetesManifest@1
              displayName: Deploy to Kubernetes cluster
              inputs:
                action: 'deploy'
                kubernetesServiceConnection: $(aksServiceConnection)
                namespace: $(kubernetesNamespace)
                manifests: |
                  $(Build.SourcesDirectory)/kubernetes/deployment.yaml
                  $(Build.SourcesDirectory)/kubernetes/service.yaml
                imagePullSecrets: |
                  $(kubernetesSecret)
                containers: |
                  $(dockerRegistryServiceConnection).azurecr.io/$(imageRepository):$(tag)
        ```
    * **Classic Editor:** Manually add tasks for Docker build/push (e.g., "Docker" task) and Kubernetes deployment (e.g., "Deploy to Kubernetes" task).
9.  **Save and Run:** Save the pipeline and run it.

### Create a CI/CD Pipeline to Build and Push a Docker Image to ACR and Deploy to ACI

1.  **Prerequisites:** Azure Container Registry (ACR) must be set up in your Azure subscription.
2.  **Service Connection:** Ensure you have a service connection to your Azure subscription.
3.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
4.  **Select Project:** Choose your project.
5.  **Go to Pipelines > Pipelines:** In the left-hand navigation, select "Pipelines" then "Pipelines".
6.  **New Pipeline:** Click "New pipeline".
7.  **Connect to Repo:** Select your code repository.
8.  **Configure Pipeline (YAML example):**
    ```yaml
    trigger:
    - main

    variables:
      dockerRegistryServiceConnection: 'YourACRServiceConnectionName' # Name of your service connection to ACR
      imageRepository: 'your-app-image'
      dockerfilePath: '$(Build.SourcesDirectory)/Dockerfile'
      tag: '$(Build.BuildId)'
      azureSubscription: 'YourAzureServiceConnectionName' # Name of your service connection
      resourceGroup: 'your-resource-group'
      containerInstanceName: 'your-aci-instance'
      containerPort: '80' # Port your application listens on

    stages:
    - stage: Build
      displayName: Build and Push Docker Image
      jobs:
      - job: BuildAndPush
        displayName: Build and Push
        pool:
          vmImage: 'ubuntu-latest'
        steps:
        - task: Docker@2
          displayName: Build and push an image to ACR
          inputs:
            containerRegistry: $(dockerRegistryServiceConnection)
            repository: $(imageRepository)
            command: 'buildAndPush'
            Dockerfile: $(dockerfilePath)
            tags: |
              $(tag)

    - stage: Deploy
      displayName: Deploy to ACI
      jobs:
      - job: DeployToACI
        displayName: Deploy to ACI
        pool:
          vmImage: 'ubuntu-latest'
        steps:
        - task: AzureCLI@2
          displayName: Deploy to Azure Container Instances
          inputs:
            azureSubscription: $(azureSubscription)
            scriptType: 'bash'
            scriptLocation: 'inlineScript'
            inlineScript: |
              az container create \
                --resource-group $(resourceGroup) \
                --name $(containerInstanceName) \
                --image $(dockerRegistryServiceConnection).azurecr.io/$(imageRepository):$(tag) \
                --dns-name-label $(containerInstanceName) \
                --ports $(containerPort) \
                --registry-login-server $(dockerRegistryServiceConnection).azurecr.io \
                --registry-username $(dockerRegistryServiceConnection) \
                --registry-password $(DOCKER_PASSWORD) # Use a pipeline variable for ACR password
    ```
9.  **Save and Run:** Save the pipeline and run it.

### Create a CI/CD Pipeline to Build a .NET Application and Deploy it to Azure App Service

1.  **Prerequisites:** An Azure App Service must be set up in your Azure subscription.
2.  **Service Connection:** Ensure you have a service connection to your Azure subscription.
3.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
4.  **Select Project:** Choose your project.
5.  **Go to Pipelines > Pipelines:** In the left-hand navigation, select "Pipelines" then "Pipelines".
6.  **New Pipeline:** Click "New pipeline".
7.  **Connect to Repo:** Select your .NET code repository.
8.  **Configure Pipeline:**
    * **YAML (Recommended):**
        ```yaml
        trigger:
        - main

        variables:
          buildConfiguration: 'Release'
          azureSubscription: 'YourAzureServiceConnectionName' # Name of your service connection
          webAppName: 'your-app-service-name'
          vmImage: 'windows-latest' # Or 'ubuntu-latest' for .NET Core on Linux

        stages:
        - stage: Build
          displayName: Build .NET Application
          jobs:
          - job: Build
            displayName: Build
            pool:
              vmImage: $(vmImage)
            steps:
            - task: UseDotNet@2
              displayName: 'Use .NET Core SDK'
              inputs:
                version: '6.x' # Or your desired .NET version
            - task: DotNetCoreCLI@2
              displayName: 'Restore'
              inputs:
                command: 'restore'
                projects: '**/*.csproj'
            - task: DotNetCoreCLI@2
              displayName: 'Build'
              inputs:
                command: 'build'
                projects: '**/*.csproj'
                arguments: '--configuration $(buildConfiguration)'
            - task: DotNetCoreCLI@2
              displayName: 'Publish'
              inputs:
                command: 'publish'
                publishWebProjects: true
                arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'
                zipAfterPublish: true
            - task: PublishBuildArtifacts@1
              displayName: 'Publish Artifacts'
              inputs:
                pathToPublish: '$(Build.ArtifactStagingDirectory)'
                artifactName: 'drop'

        - stage: Deploy
          displayName: Deploy to Azure App Service
          jobs:
          - deployment: Deploy
            displayName: Deploy
            environment: 'YourAppServiceEnvironment' # Optional: if you have an environment in Azure DevOps
            pool:
              vmImage: $(vmImage)
            strategy:
              runOnce:
                deploy:
                  steps:
                  - task: AzureWebApp@1
                    displayName: 'Deploy Azure Web App'
                    inputs:
                      azureSubscription: $(azureSubscription)
                      appName: $(webAppName)
                      package: '$(Pipeline.Workspace)/drop/**/*.zip'
        ```
    * **Classic Editor:** Use "Azure App Service deploy" task.
9.  **Save and Run:** Save the pipeline and run it.

### Create a CI/CD Pipeline to Build a React Application and Deploy it to an Azure Virtual Machine

1.  **Prerequisites:** An Azure Virtual Machine (VM) with a web server (e.g., Nginx, Apache, IIS) configured to serve static files.
2.  **Service Connection:** Ensure you have a service connection to your Azure subscription.
3.  **Navigate to Azure DevOps:** Go to your Azure DevOps organization.
4.  **Select Project:** Choose your project.
5.  **Go to Pipelines > Pipelines:** In the left-hand navigation, select "Pipelines" then "Pipelines".
6.  **New Pipeline:** Click "New pipeline".
7.  **Connect to Repo:** Select your React code repository.
8.  **Configure Pipeline (YAML example):**
    ```yaml
    trigger:
    - main

    variables:
      buildConfiguration: 'production' # Or 'development'
      azureSubscription: 'YourAzureServiceConnectionName' # Name of your service connection
      resourceGroup: 'your-vm-resource-group'
      vmName: 'your-linux-vm-name' # Or 'your-windows-vm-name'
      deployPath: '/var/www/html' # For Linux, e.g., Nginx default path. Adjust for Windows/IIS.
      vmUser: 'your-vm-username'

    stages:
    - stage: Build
      displayName: Build React Application
      jobs:
      - job: Build
        displayName: Build
        pool:
          vmImage: 'ubuntu-latest' # Or 'windows-latest'
        steps:
        - task: Npm@1
          displayName: 'Install dependencies'
          inputs:
            command: 'install'
        - task: Npm@1
          displayName: 'Build React App'
          inputs:
            command: 'custom'
            customCommand: 'run build' # Assumes 'build' script in package.json
            arguments: '--configuration=$(buildConfiguration)'
        - task: PublishBuildArtifacts@1
          displayName: 'Publish Artifacts'
          inputs:
            pathToPublish: '$(System.DefaultWorkingDirectory)/build' # Assuming 'build' folder for React
            artifactName: 'drop'

    - stage: Deploy
      displayName: Deploy to Azure VM
      jobs:
      - deployment: Deploy
        displayName: Deploy
        pool:
          vmImage: 'ubuntu-latest' # Or 'windows-latest'
        environment: 'YourVMEnvironment' # Optional: if you have an environment in Azure DevOps
        strategy:
          runOnce:
            deploy:
              steps:
              - task: DownloadBuildArtifacts@1
                displayName: 'Download Build Artifacts'
                inputs:
                  artifactName: 'drop'
                  pathToDownload: '$(System.DefaultWorkingDirectory)/_downloaded_artifacts'

              - task: SSH@0 # For Linux VM deployment
                displayName: 'Copy files to VM via SSH'
                inputs:
                  sshEndpoint: 'YourSSHServiceConnectionName' # Create an SSH service connection to your VM
                  sourceFolder: '$(System.DefaultWorkingDirectory)/_downloaded_artifacts/drop'
                  targetFolder: '$(deployPath)'
                  overwrite: true

              # For Windows VM deployment (using Azure File Copy or PowerShell on Target Machines)
              # - task: AzureFileCopy@4
              #   displayName: 'Azure File Copy to VM'
              #   inputs:
              #     SourcePath: '$(System.DefaultWorkingDirectory)/_downloaded_artifacts/drop'
              #     azureSubscription: $(azureSubscription)
              #     Destination: 'AzureVMs'
              #     MachineNames: '$(vmName)'
              #     AdminUserName: '$(vmUser)'
              #     AdminPassword: '$(vmPassword)' # Use a pipeline variable for VM password
              #     TargetPath: '$(deployPath)'
    ```
9.  **Save and Run:** Save the pipeline and run it.