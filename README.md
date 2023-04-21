# Tekton Repo

## Instalación de herramientas necesarias
## Tekton

```sh
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/dashboard/latest/release-full.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml
```

### Monitorizar instalación
```sh
kubectl get pods -n tekton-pipelines --watch
```
### Instalar Git Clone y Kaniko de Tekton Hub
```sh
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/kaniko/0.6/kaniko.yaml
```
### Port Forward Tekton Dashboard a localhost
```sh
kubectl port-forward -n tekton-pipelines svc/tekton-dashboard 9097:9097 --address 0.0.0.0
```

## ArgoCD
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Port Forward ArgoCD Server a localhost
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:80 --address 0.0.0.0
```

### Coger Password Inicial para acceder a ArgoCD
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Loguearse con las credenciales:\
user: admin\
pass: contraseña del paso anterior

### Instalar los recursos necesarios 

- Docker Secret
```sh
kubectl create secret docker-registry docker-credentials --docker-username=<username> --docker-password=<password>
```
- Github Secret\
Usar la plantilla en secrets/github-secret.yaml sustituyendo nuestros datos

```sh
kubectl create -f tasks.yaml,pipeline.yaml
```

### Lanzar un PipelineRun de prueba
```sh
kubectl create -f pipelinerun.yaml
```

## Triggers

### Crear recursos
```sh
kubectl create -f triggers-rbac.yaml
kubectl create -f event-listener.yaml,trigger-template.yaml,trigger-binding.yaml
```

### Port forward EventListener Servicio al localhost (asegurarse de que el puerto no está ya en uso)
```sh
kubectl port-forward svc/el-tekton-ejemplo-eventlistener 8080
```

### Exponer a internet usando ngrok
```sh
ngrok http 8080
```

### Añadir ngrok URL al repo de Github (Settings > Webhooks > Add webhook)

### Realizar un test del trigger haciendo un push de un cambio en la app (index.html)