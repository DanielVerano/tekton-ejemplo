# Tekton Repo

## Git Clone
```sh
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
```

## Kaniko
```sh
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/kaniko/0.6/kaniko.yaml
```

## Triggers

Create resources
```sh
kubectl create -f triggers-rbac.yaml
kubectl create -f event-listener.yaml,trigger-template.yaml,trigger-binding.yaml
```

Port forward EventListener Service to localhost
```sh
kubectl port-forward svc/el-tekton-ejemplo-eventlistener 8080
```

Expose to public with ngrok
```sh
ngrok http 8080
```

Add ngrok URL to Github repo (Settings > Webhooks)

Test Trigger by making a change in the app (index.html)