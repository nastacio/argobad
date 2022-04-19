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

## PostSync Hook

```yaml
---
apiVersion: batch/v1
kind: Job
metadata:
  name: postsync-1
  annotations:
    argocd.argoproj.io/hook: PostSync
  namespace: myworkload
spec:
  template:
    spec:
      containers:
        - name: config
          image: quay.io/openshift/origin-cli:latest
          imagePullPolicy: IfNotPresent
          command:
            - /bin/sh
            - -c
            - |
              set -eo pipefail
              set -x
              
              oc get Secret -n openshift-gitops

      restartPolicy: Never
      serviceAccountName: default
  backoffLimit: 1
```


## Examples of Argo CD role admin

### Local namespace

```yaml
   
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: null
  name: argo-bad-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: admin
subjects:
  - kind: ServiceAccount
    name: openshift-gitops-argocd-application-controller
    namespace: openshift-gitops
```

### Global cluster

```yaml
   
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  annotations:
    argocd.argoproj.io/sync-wave: "10"
  creationTimestamp: null
  name: cp4d-cluster-roles
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: default
    namespace: ibm-common-services
  - kind: ServiceAccount
    name: "{{.Values.serviceaccount.argocd_application_controller}}"
    namespace: "{{.Values.metadata.argocd_namespace}}"
```
