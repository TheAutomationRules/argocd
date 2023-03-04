# DOCKER Construcción de la imágen

Para hacer la build con tag 0.0.1
```
docker build -t ghcr.io/theautomationrules/nginx-demo-arm:0.0.1 docker/nginx
```
Para hacer la build con tag 0.0.1
```
docker build --build-arg FROM_IMG="nginx" --build-arg FROM_VER="1.22.1" --build-arg VERSION="0.0.1" -t ghcr.io/theautomationrules/nginx-demo-arm:0.0.1 docker/nginx
```
Para lanzar un contenedor mapeando el puerto local 8081 al 80 del contenedor
```
docker run -d --name nginx-demo-arm -p 8081:80 ghcr.io/theautomationrules/nginx-demo-arm:0.0.1
```
Para subir la imagen a nuestro registry
```
docker push ghcr.io/theautomationrules/nginx-demo-arm:0.0.1
```
Para parar el contenedor
```
docker stop nginx-demo-arm
```
Para eliminar el contenedor
```
docker rm nginx-demo-arm
```
Ahora vamos a generar una nueva version 0.1.1 y siguientes versiones
```
VERSION=0.1.1 && \
    docker build --build-arg FROM_IMG="nginx" --build-arg FROM_VER="1.22.1" --build-arg VERSION="$VERSION" -t ghcr.io/theautomationrules/nginx-demo-arm:$VERSION docker/nginx && \
    docker push ghcr.io/theautomationrules/nginx-demo-arm:$VERSION
```