# argobad

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
    name: ''
    namespace: myworkload
    server: 'https://kubernetes.default.svc'
  source:
    path: config/argocd
    repoURL: 'https://github.com/nastacio/argobad'
    targetRevision: main
  project: default
  syncPolicy:
    automated:
      prune: false
      selfHeal: false
    syncOptions:
      - CreateNamespace=true
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
