
# DevOps 2024 - Kubernetes Deployment

Este documento describe cómo desplegar tu aplicación en Kubernetes utilizando Minikube. La aplicación consta de un backend y un frontend, cuyas imágenes están disponibles en Docker Hub.

## Prerrequisitos

- Tener instalado [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- Tener instalado [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Pasos para desplegar la aplicación

### 1. Iniciar Minikube

```sh
minikube start
```

### 2. Crear los archivos de despliegue

#### backend-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: zape/devops2024-backend:latest
        ports:
        - containerPort: 8000
```

#### backend-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 8000
      targetPort: 8000
```

#### frontend-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: zape/devops2024-frontend:latest
        ports:
        - containerPort: 3000
```

#### frontend-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
```

#### ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: frontend-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: frontend.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 3000
```

### 3. Aplicar los archivos de despliegue

```sh
kubectl apply -f backend-deployment.yaml
kubectl apply -f backend-service.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
kubectl apply -f ingress.yaml
```

### 4. Habilitar el addon de ingress en Minikube

```sh
minikube addons enable ingress
```

### 5. Añadir la entrada al archivo hosts

Añadir la siguiente línea al archivo `/etc/hosts`:

```sh
127.0.0.1 frontend.local
```

### 6. Acceder a la aplicación

Abre tu navegador y visita `http://frontend.local`.