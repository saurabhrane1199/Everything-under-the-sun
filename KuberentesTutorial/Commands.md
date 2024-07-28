```bash

## Build Images from Dockerfile and Docker-Compose
docker build -t cattle-viz-backend:latest ./backend

## View all images in local repository
docker images

## Build Images from Dockerfile and Docker-Compose
docker build -t cattle-viz-frontend:latest ./ui 

## Deploy to cluster
kubectl apply -f backend-deployment.yaml 


## Push to dockerhub
docker tag cattle-viz-frontend:latest saurabhrane1199/cattle-viz-frontend:latest

docker push saurabhrane1199/cattle-viz-backend:latest

```