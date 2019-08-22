I like to describe how I think a Multi-Team k8s-Cluster could be managed with ArgoCD.

First of all these are my goals:

- easy bootstrapping of a new Cluster
- use ArgoCD also for Disaster/Recovery
- easy "application onboarding" of new applications but with necessary controls of compliance and policies
- completely self-service and freedom for dev-teams, as long as no other team gets hurt

