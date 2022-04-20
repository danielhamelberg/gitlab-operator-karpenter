# Prerequisites
Install cert-manager:

`kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.7.1/cert-manager.yaml`

# Usage
To link a GitLab Runner instance to a self-hosted GitLab instance or the hosted GitLab, you first need to:

Create a secret containing the runner-registration-token from your GitLab project.
```
cat > gitlab-runner-secret.yml << EOF
apiVersion: v1
kind: Secret
metadata:
  name: gitlab-runner-secret
type: Opaque
stringData:
  runner-registration-token: REPLACE_ME # your project runner secret
EOF
kubectl apply -f gitlab-runner-secret.yml
```

Create the Custom Resource Definition (CRD) file and include the following information. The tags value must be openshift for the job to run.
```
cat > gitlab-runner.yml << EOF
 apiVersion: apps.gitlab.com/v1beta2
 kind: Runner
 metadata:
   name: gitlab-runner
 spec:
   gitlabUrl: https://gitlab.example.com
   buildImage: alpine
   token: gitlab-runner-secret
   tags: openshift
 EOF
kubectl apply -f gitlab-runner.yml
```
