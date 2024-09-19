# Despliegue Continuo con ArgoCD en Minikube

Este documento describe cómo configurar e instalar ArgoCD en Minikube para realizar despliegues continuos utilizando los manifiestos del repositorio `bzappellini/devops2024` en GitHub.

## Prerrequisitos

- Minikube instalado y en funcionamiento.
- kubectl configurado para interactuar con Minikube.
- Git instalado.

## Instalación de ArgoCD

1. **Instalar ArgoCD en el clúster de Minikube:**

    ```sh
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

2. **Instalar la CLI de ArgoCD:**

    ```sh
    # En Linux
    sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
    sudo chmod +x /usr/local/bin/argocd

    # En macOS
    brew install argocd
    ```

3. **Configurar el acceso a la interfaz web de ArgoCD:**

    ```sh
    kubectl port-forward svc/argocd-server -n argocd 8443:443
    ```

4. **Obtener la contraseña inicial de ArgoCD:**

    ```sh
    kubectl get pods -n argocd
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```

    Accede a la interfaz web en `https://localhost:8443` con el usuario `admin` y la contraseña obtenida.

## Configuración de ArgoCD

1. **Conectar ArgoCD a tu repositorio de GitHub:**

    ```sh
    argocd repo add https://github.com/bzappellini/devops2024 --username <tu-usuario> --password <tu-contraseña>
    ```

2. **Crear una aplicación en ArgoCD:**

    ```sh
    argocd app create devops2024-app \
      --repo https://github.com/bzappellini/devops2024 \
      --path 4-kubernetes \
      --dest-server https://kubernetes.default.svc \
      --dest-namespace default
    ```

3. **Sincronizar la aplicación:**

    ```sh
    argocd app sync devops2024-app
    ```

## Verificación

1. **Verificar el estado de la aplicación:**

    ```sh
    argocd app get devops2024-app
    ```

2. **Acceder a la interfaz web de ArgoCD para monitorear el despliegue.**

## Conclusión

Siguiendo estos pasos, habrás configurado ArgoCD en Minikube y desplegado los manifiestos del repositorio `bzappellini/devops2024`. Ahora puedes disfrutar de despliegues continuos automatizados.
