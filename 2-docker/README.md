## Crear una imagen de docker para el backend
```bash
docker build -t devops2024-backend:latest backend/
```

## Crear una imagen de docker para el frontend
```bash
docker build -t devops2024-frontend:latest frontend/
```
## subir la imagen a docker hub
```bash
docker login
docker tag devops2024-backend:latest <tu-usuario>/devops2024-backend:latest
docker tag devops2024-frontend:latest <tu-usuario>/devops2024-frontend:latest
docker push <tu-usuario>/devops2024-backend:latest
docker push <tu-usuario>/devops2024-frontend:latest
```

## Correr el backend
```bash
docker run -d -p 8000:8000 devops2024-backend:latest
```

## Correr el frontend
```bash
docker run -d -p 3000:3000 devops2024-frontend:latest
```
## para detener los contenedores
```bash
docker stop $(docker ps -q)
```
## para eliminar todos los contenedores
```bash
docker rm $(docker ps -a -q)
```
## para eliminar las imagenes
```bash
docker rmi devops2024-backend:latest devops2024-frontend:latest
```
# Docker compose

## Correr todo con docker compose
```bash
docker compose up --build
```
