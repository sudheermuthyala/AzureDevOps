## What is azure devops 
- technological implementation
- 5 
```t
## Stages
## stage
## Jobs
## Job-1
## steps
## step-1
## Task-1
## Task-2
## TasK-3
## NOTE-1: we have a two types of "Jobs"

stages: # Stage one
- stage-1: 'Build'
  jobs:
    - job1:
      steps:
      - step-1:
        - task-1:
        - task-2:
        - task-3:
    - job2:
      steps:
      - step-1:
        - task-1:
        - task-2:
        - task-3:

- stage-2: 'Release' # Stage Two      Release or Deployment Stage
  jobs:
    - job2:
      steps:
      - step-2:
        - task-1:
        - task-2:
        - task-3:

## We can define all tasks Under single "steps"
## EX: 
stages:
- stage: 'Build'
  jobs:
  - job-1:
    steps:
      - task1:
      - task2:
      - task3:
      - task4:
      - task5:
      - task6:
   

stages:
  - stage-1: 'Build-1'
    jobs:
      - job1:
        steps:
        - step-1:
          - task-1:
          - task-2:
          - task-3:
  - stage-2:
```

## Types of Jobs
- Build jobs or standard job
  - Reference from AzureDevops Portal to Reffe [Treditional](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) Jobs
```t
stages: 
- stage-1: 'Build or Standard jobs'
  jobs:
  - job: string  # name of the job, A-Z, a-z, 0-9, and underscore
    displayName: string  # friendly name to display in the UI
    dependsOn: string | [ string ]
    condition: string
    strategy:
      parallel: # parallel strategy
      matrix: # matrix strategy
      maxParallel: number # maximum number simultaneous matrix legs to run
      # note: `parallel` and `matrix` are mutually exclusive
      # you may specify one or the other; including both is an error
      # `maxParallel` is only valid with `matrix`
    continueOnError: boolean  # 'true' if future jobs should run even if this job fails; defaults to 'false'
    pool: pool # agent pool
    workspace:
      clean: outputs | resources | all # what to clean up before the job runs
    container: containerReference # container to run this job inside
    timeoutInMinutes: number # how long to run the job before automatically cancelling
    cancelTimeoutInMinutes: number # how much time to give 'run always even if cancelled tasks' before killing them
    variables: { string: string } | [ variable | variableReference ] 
    steps: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
    services: { string: string | container } # container resources to run as a service container
    uses: # Any resources (repos or pools) required by this job that are not already referenced
      repositories: [ string ] # Repository references to Azure Git repositories
      pools: [ string ] # Pool names, typically when using a matrix strategy for the job
```
  
<p align="center">
  <img src="https://github.com/sudheermuthyala/AzureDevOps/blob/main/2022-12-26-13-52-06.png" />
    </p>
    
- Release or Deployment Jobs [ReferenceT=58:55](https://youtu.be/gXk_GicAzeA?list=PLKb_hnKdTrx1Mb5Gr_o7Cnwz3hCh-vx4r)
  - Reference from AzureDevops Portal to Reffer [Deployment jobs](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops)


```t
- stage-2: "Release or Deployment Jobs"
  jobs:
  - deployment: string   # name of the deployment job, A-Z, a-z, 0-9, and underscore. The word "deploy" is a keyword and is unsupported as the deployment name.
    displayName: string  # friendly name to display in the UI
    pool:                # not required for virtual machine resources
      name: string       # Use only global level variables for defining a pool name. Stage/job level variables are not supported to define pool name.
      demands: string | [ string ]
    workspace:
      clean: outputs | resources | all # what to clean up before the job runs
    dependsOn: string
    condition: string
    continueOnError: boolean                # 'true' if future jobs should run even if this job fails; defaults to 'false'
    container: containerReference # container to run this job inside
    services: { string: string | container } # container resources to run as a service container
    timeoutInMinutes: nonEmptyString        # how long to run the job before automatically cancelling
    cancelTimeoutInMinutes: nonEmptyString  # how much time to give 'run always even if cancelled tasks' before killing them
    variables: # several syntaxes, see specific section
    environment: string  # target environment name and optionally a resource name to record the deployment history; format: <environment-name>.<resource-name>
    strategy:
      runOnce:    #rolling, canary are the other strategies that are supported
        deploy:
          steps: [ script | bash | pwsh | powershell | checkout | task | templateReference ]

```
There is a more detailed, alternative syntax you can also use for the environment property.
```t
environment:
    name: string # Name of environment.
    resourceName: string # Name of resource.
    resourceId: string # Id of resource.
    resourceType: string # Type of environment resource.
    tags: string # List of tag filters.
```
<p align="center">
  <img src="https://github.com/sudheermuthyala/AzureDevOps/blob/main/2022-12-26-13-55-10.png" />
    </p>

Deployment strategy [Reference](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops#deployment-strategies)
- **runOnce** [RunOnce deployment strategy](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops#runonce-deployment-strategy)
  - runOnce is the simplest deployment strategy wherein all the lifecycle hooks, namely preDeploy deploy, routeTraffic, and postRouteTraffic, are executed once. Then, either on: success or on: failure is executed.

```t
strategy: 
    runOnce:
      preDeploy:        
        pool: [ server | pool ] # See pool schema.        
        steps:
        - script: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
      deploy:          
        pool: [ server | pool ] # See pool schema.        
        steps:
        ...
      routeTraffic:         
        pool: [ server | pool ]         
        steps:
        ...        
      postRouteTraffic:          
        pool: [ server | pool ]        
        steps:
        ...
      on:
        failure:         
          pool: [ server | pool ]           
          steps:
          ...
        success:          
          pool: [ server | pool ]           
          steps:
          ...
```
Rolling deployment strategy
```
A rolling deployment replaces instances of the previous version of an application with instances of the new version of the application on a fixed set of virtual machines (rolling set) in each iteration.

We currently only support the rolling strategy to VM resources.

For example, a rolling deployment typically waits for deployments on each set of virtual machines to complete before proceeding to the next set of deployments. You could do a health check after each iteration and if a significant issue occurs, the rolling deployment can be stopped.

Rolling deployments can be configured by specifying the keyword rolling: under the strategy: node. The strategy.name variable is available in this strategy block, which takes the name of the strategy. In this case, rolling.
```
```t
strategy:
  rolling:
    maxParallel: [ number or percentage as x% ]
    preDeploy:        
      steps:
      - script: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
    deploy:          
      steps:
      ...
    routeTraffic:         
      steps:
      ...        
    postRouteTraffic:          
      steps:
      ...
    on:
      failure:         
        steps:
        ...
      success:          
        steps:
        ...

```

```t
stages: 
- stage-1: 'Build or Standard jobs'
  jobs:
    - job1: ## for Buld jobs we can define job hear it is a build job 
      steps:
      - step-1:
        - task-1:
        - task-2:
        - task-3:
    - job2:
      steps:
      - step-1:
        - task-1:
        - task-2:
        - task-3:

- stage-2: "Release or Deployment Jobs"
  jobs:
  - deployment:
```

## Deployment Groups 
what is A Deploymet Groups 
- Meachions can with in AzureDevops or Outof AzureDevops (Ex:AWS,GCP)
- For Deployments we have two options in ADO
    - Environments : Is also similar to Deployment Groups. we only prefer resources with in ADO 
    - Deployment Groups: it can be any thins like it will be in ADO,AWS,AZURE,outeof azure vn, or onprimesis
To Connectect To ADO and intrect with ADO in the control of ADO 

## Azurepredefined Varibles
[Runtime parameters](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runtime-parameters?view=azure-devops&tabs=script)

## Build.StagingDirectory

## Build.ArtifactStagingDirectory