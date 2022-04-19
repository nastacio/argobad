# argohooks# argobad

Example of application to add to Argo CD

```yaml
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  creationTimestamp: null
  name: argo-bad-app
  namespace: openshift-gitops
spec:
  destination:
    namespace: openshift-gitops
    server: https://kubernetes.default.svc
  project: default
  source:
    helm:
      parameters:
    path: config/argocd
    repoURL: https://github.com/nastacio/argobad
    targetRevision: main
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
status:
  health: {}
  summary: {}
  sync:
    comparedTo:
      destination: {}
      source:
        repoURL: ""
    status: ""
```
