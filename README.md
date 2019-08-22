I like to describe how I think a Multi-Team k8s-Cluster could be managed with ArgoCD.

First of all these are my goals:

- easy bootstrapping of a new Cluster
- use ArgoCD also for Disaster/Recovery
- easy "application onboarding" of new applications but with necessary controls of compliance and policies
- completely self-service and freedom for dev-teams, as long as no other team gets hurt
- creating namespaces is something the platform-team needs to do, because of default-policies, security and resource-quotas. however, thats maybe something to discuss in the future.

How it may look like:

https://github.com/tschonnie/argocd-pipeline/blob/master/argocd_objects_repos.png

Procedures:

1. The first step for a new k8s-Cluster would be:
  1.1. install ArgoCD
  1.2. install bootstrap application with "kubectl apply -f <gitrepo>/k8s-bootstrap-application.yaml"
  from this point on everything in the k8s-platform.git repo gets synced
  
2. each dev-team which wants to host applications on this cluster must:
  2.1 merge a <team>-applications.yaml in the k8s-platform.git repo (must be accepted by the platform team)
  
3. for each application the dev-team must:
  2.1 merge a <team>-namespaces.yaml in the k8s-platform.git repo for the new namespace (must be accepted by the platform team)
  2.2 merge <team>-project.yaml which contains the new <application>-infra repo und destination namespace in the k8s-platform.git repo  (must be accepted by the platform team)
  2.3 create <application>.yaml in the <team>-application.git
  2.4 create <application>-infra.git repo where the actual infrastructure is defined

Somehow it currently seems that there are too many merge-requests per application, which a platform-team must accept. There is some room for improvement.

Questions:

- could a dev-team create applications in a <team>-application.git repo if the super application <team>-application doesn't define the argocd namespace as a destination namespace?
- is it possible that applications in a <team>-application.git repo reference other projects than the super application <team>-application itself? If so, each dev-team can define its own unrestricted project-definitions and use this as a project. would be a bad security issue.
- could teams create argocd repository objects in their own <team>-application.git repo? otherwise each new repo for a new application must be merged and accepted by the platform-team.
- is it possible to use regular expressions in the <team>-project.yaml for the source repository? otherwhise each no repo for an app must be merged and accepted by the platform-team.
