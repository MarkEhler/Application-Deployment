# Application Deployment
## Summary
This repository contains the following: 
- manifests that are used to deploy the Application components to our kubernetes clusters
- manifests that are used to deploy the tools which support Application
- automation which performs these deployments

## Automation
This repo is updated by the Application component repos (front end, back end, etc.) upon merges into their own develop branches.
## Legend
```mermaid
flowchart TD
   A{AKS Cluster}
   B{{Automation Tool}}
   C(Branch)
   D[File]
   E[[Person]]
```
### Dev deployments
```mermaid
flowchart TD
   A(Frontend/feature)-->|merge|B(Frontend/develop)
   B --> |automatic run|C{{Frontend GitHub Action}}
   C --> |update image|D[Application-deployment/../Frontend/../dev/kustomize.yaml ]
   E{{Argo CD/dev}} --> |watches|D
   E --> |update image| F{dev aks cluster}
```

### QA deployments
```mermaid
flowchart TD
   A(Frontend/develop)-->|merge|B(Frontend/release/v1.0)
   B --> |automatic run|C{{Frontend GitHub Action}}
   C --> |update image|D[Application-deployment/../Frontend/../qa/kustomize.yaml ]
   E{{Argo CD/qa}} --> |watches|D
   E --> |update image| F{qa aks cluster}
```

### Prod Deployments
```mermaid
flowchart TD
    A[[DevOps Engineer]] --> |manual run|C{{Application-deployment GitHub Action}}
    C --> |get sha|B(Frontend/release/v1.0)
    C --> |update image|D[Application-deployment/../Frontend/../prod/kustomize.yaml ]
    E{{Argo CD/prod}} --> |watch|D
    E --> |update image| F{prod aks cluster}
```
