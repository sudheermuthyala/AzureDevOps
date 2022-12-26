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
  - Reference from AzureDevops Portal to Reffe [Treditional](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml)
- Release or Deployment Jobs [ReferenceT=58:55](https://youtu.be/gXk_GicAzeA?list=PLKb_hnKdTrx1Mb5Gr_o7Cnwz3hCh-vx4r)
  - Reference from AzureDevops Portal to Reffer [Deployment jobs](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops)

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