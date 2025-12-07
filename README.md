# Sources

- https://www.youtube.com/watch?v=FcBs2iwXC-U
- https://opengitops.dev/

# What is GitOps?

In short, the idea is to manage our K8s clusters through code, using a Git repo as our **single source of truth**.  

In a K8s cluster, there are several types of resources (all defined as code):
- the infrastructure: monitoring tooling, ingress controller, database controller, etc.
- the application definition: a set of containerized workloads grouped into logical units like Pods, Deployments, and Services

GitOps is a set of best practices where the entire code delivery process is controlled via Git, including: 
- infrastructure as code,
- application definition as code,
- automation to complete updates and rollbacks (Renovate).

# The Key GitOps Principles

- The entire system (infrastructure and applications) is described **declaratively**
- The canonical desired system state is versioned in Git
- Approved changes are automated and applied to the system
- Software agents ensure correctness and alert on divergence

# What happens in a GitOps workflow?

1. We have a K8S cluster running on our PC (be it a laptop, a Raspberry Pi, or any computer)
2. We use Flux or Argo CD as a controller for our Kubernetes cluster.
3. This controller is constantly watching for any change in our Git repository = **Reconciliation Loop**
4. When a change is made in the Git repo, Flux|Argo are going to implement that change in our cluster

In other words, we define a desired state in our Git repo, and we tell the controller to apply that state to our cluster.  
<img width="620" height="504" alt="image" src="https://github.com/user-attachments/assets/54da41b3-6e76-4d38-a868-e487fa8dc4d3" />  

The way GitOps works implies that there's no direct access to our K8s cluster. Instead, we only need access to the Git repo.  
Which allows any member of our team to send pull requests (**PRs**) to the repo in order to trigger an update in our cluster.  

## How do Renovate and the GitOps Controller work? 

Pull Requests (PRs) are not actually sent by humans, we have an automation running that checks the tags (versions) in our Git repo.  
This monitoring is done through a tool called "**Renovate**".  

Renovate is a **dependency-updater bot** that runs against our Git repository.  
It scans manifests (Kubernetes YAML, Helm values, Kustomize, etc.) for image tags, Helm chart versions, or other dependencies.  
When a newer version is available, it opens a **pull/merge** request that updates the tag or version in the Git repo.  

As previously explained, in a GitOps workflow, Flux or Argo CD are the GitOps controllers that reconcile the cluster state with the Git repo.  
When a PR is approved and merged, the Git repo now declares a new version.  
The GitOps controller detects this change and updates the cluster accoridngly.  

More concisely:
- Renovate detects a new version → opens a PR in Git.
- Our team reviews and approves/merges the PR.
- Flux/Argo CD observes the merged change in Git and updates the cluster to match that new version

# Differences between Flux and Argo CD



16/32
