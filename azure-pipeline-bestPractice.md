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
```

## NOTE-POINTS:
- We have a two types of "Jobs"
    - standard Jobs
        - Regular Build Jobs 
    - Deployment Jobs


```t
stages:
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
  - stage-2: 'Release'
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