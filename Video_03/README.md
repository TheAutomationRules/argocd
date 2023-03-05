# Automatiza ⚙️ los despliegues con ArgoCD 🐙 Image Updater ✅

Hoy os voy a mostrar como automatizar la actualizacion de versiones de las aplicaciones desplegadas en tu cluster 
kubernetes desde ArgoCD con ArgoCD Image Updater.

**Video:**

0:00 Inicio

0:13 Intro

0:24 Explicación

3:14 DEMO

18:43 Conclusión

20:00 Despedida

20:10 Outro


GitHub:
https://github.com/TheAutomationRules/argocd/blob/main/Video_03/README.md

Twitter: @TheAutomaRules
Instagram: TheAutomationRules

#kubernetes #argocd #image #updater #gitops #minikube #continuousdelivery

---

## Instalación basado en el video anterior.

Para instalar ArgoCD seguiremos los mismos pasos que en nuestros videos anteriores, para ello usaremos el directorio 
de instalación del video anterior parados dentro de la raiz del directorio Video_03.
````
helm install argo-cd argo-cd/ \
      --namespace argocd \
      --create-namespace --wait \
      -f ../Video_02/argo-cd/values-custom.yaml
````
Creamos un proyecto de pruebas llamado argocd-demo
````
kubectl apply -f argocd-manifests/project-argocd-demo.yaml
````
Creamos un namespace de pruebas llamado argocd-demo
````
kubectl create ns argocd-demo
````
Creamos un secret con el Token de GitHub con permisos packages:read en el namespace argocd-demo con el formato 
username:token y en base64, es el token que usara ArgoCD Image Updater para conectar con el registry y hacer pull de 
la imagen.
````
kubectl apply -f k8s/git-secret.yaml
````
Ahora levantamos un port-forward para acceder a ArgoCD
````
kubectl port-forward service/argo-cd-argocd-server -n argocd 8080:443
````
Para instalar ArgoCD Image Updater añadimos el repo de Helm de ArgoCD Image Updater
```
helm repo add argo https://argoproj.github.io/argo-helm
```
Ahora hacemos un pull del Helm Chart de instalación de Image Updater, lo he ubicado en este mismo repo en 
official/argocd-image-updaterya me envio 
````
helm pull argo/argocd-image-updater argo/argocd-image-updater --version 0.8.1
````
Duplicamos el archivo values.yaml y lo renombramos como values-custom.yaml, lo importante que tenemos que modificar 
es en la línea 101 (registries), necesitamos añadir nuestro registry, en mi caso voy a usar el GitHub Container 
Registry, ejemplo:
````
  registries: #[]
    - name: GitHub Container Registry
      prefix: ghcr.io
      api_url: https://ghcr.io
      ping: no
      insecure: true
      credentials: secret:argocd/git-credentials#creds
````
Instalamos ahora si el Image Updater pero lo hacemos desde una aplicacion de ArgoCD por supuesto:
```
kubectl apply -f argocd-manifests/image-updater.yaml
```
Ahora instalamos nuestra aplicacion DEMO que no es mas que un NGiNX customizado que hemos generado con el Dockerfile 
que tenemos en el directorio docker/nginx/Dockerfile, y obviamente tambien lo hacemos con una aplicacion de ArgoCD 😎
```
kubectl apply -f argocd-manifests/nginx-demo-arm.yaml
```
Levantamos un port-forward para que podamos acceder al NGiNX DEMO
````
kubectl port-forward service/nginx-demo-arm -n argocd-demo 8082:80
````

## Peeeeero! lo podemos simplificar instalando directamente desde el Makefile

Instalamos ArgoCD
````
make start
````
Instalamos ArgoCD Image Updater
````
make installiu
````
Accedemos a la App DEMO
````
make forward-app
````
Y ESO ES TODO!