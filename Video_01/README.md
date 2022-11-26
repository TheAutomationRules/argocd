# Primeros pasos 🏃‍️ con ArgoCD en Kubernetes 🤘

## Instalación con Helm Chart y gestion desde CLI

---
## Requisitos:

- Tener instalado el binario de Helm https://helm.sh/docs/intro/install/
- Tener instalado Minikube https://minikube.sigs.k8s.io/docs/start/
- Tener instalado el Binario de ArgoCD https://argo-cd.readthedocs.io/en/stable/cli_installation/
---
Iniciamos nuestro Minikube para contar con un Cluster de K8s en local
````
minikube start
````
Añadimos el repo de Helm
```
helm repo add argo https://argoproj.github.io/argo-helm
```
Hacemos pull del Chart para descargarlo, poder ver el contenido del Chart e instalarlo.
```
helm pull argo/argo-cd --version 5.8.2
```
Descomprimimos el paquete TGZ del Chart descargado
````
tar -zxvf argo-cd-5.8.2.tgz
````
Hacemos la instalación pasando parámetros de configuración
````
helm install argo-cd argo-cd/ \
  --namespace argocd \
  --create-namespace --wait \
  --set configs.credentialTemplates.github.url=https://github.com/TheAutomationRules \
  --set configs.credentialTemplates.github.username=$(cat ~/.secrets/github/theautomationrules/user) \
  --set configs.credentialTemplates.github.password=$(cat ~/.secrets/github/theautomationrules/token)
````
Imprimimos en pantalla la contraseña del usuario "admin" por defecto que se ha generado automáticamente en la instalación
````
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
````
Levantamos un Port-Forward para poder acceder a ArgoCD UI desde localhost:8080
````
kubectl port-forward service/argo-cd-argocd-server -n argocd 8080:443
````
Hacemos login en ArgoCD con la contraseña que hemos obtenido
````
argocd login localhost:8080
````
Una vez que hemos hecho login satisfactoriamente cambiamos la contraseña generada por una que nos venga mejor, como por ejemplo "argocdpass"
````
argocd account update-password
````
Ahora que estamos logados con el binario de argocd podemos crear el repositorio de código.
````
argocd repo add https://github.com/TheAutomationRules/argocd.git
````
Creamos un proyecto de pruebas en el que solo se puedan crear aplicaciones en el namespace "testing" y con determinado repositorio de código
````
argocd proj create testing -d https://kubernetes.default.svc,testing -s https://github.com/TheAutomationRules/argocd.git
````
Si quisieramos crear un proyecto en el que esté permitido cualquier namespace y cualquier repositorio
````
argocd proj create system-admin -d https://kubernetes.default.svc,* -s '*'
````
Creamos el Namespace "testing" que sera el que usaremos para desplegar las aplicaciones
````
kubectl create ns testing
````
Ahora creamos nuestra primera aplicación de pruebas en el proyecto que hemos creado anteriormente
````
argocd app create guestbook \
  --repo https://github.com/TheAutomationRules/argocd.git \
  --revision main --path ./official/examples/guestbook \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace testing \
  --project testing
````
Ahora creamos otra app, pero esta vez con sincronización automática
````
argocd app create helm-guestbook \
  --repo https://github.com/TheAutomationRules/argocd.git \
  --revision main --path ./official/examples/helm-guestbook \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace testing \
  --sync-policy automated \
  --project testing
````
Ahora podemos relanzar la primera App y sincronizar desde la CLI
````
argocd app sync guestbook
````
Si queremos saber el estatus de la App
````
argocd app get guestbook
````
Ahora para eliminar las Apps
````
argocd app delete guestbook
````
Podemos listar los Charts que tenemos instalados
````
helm list -n argocd
````
Desinstalamos todo!
````
helm uninstall argo-cd -n argocd
````
Ahora podemos eliminar los Namespaces que hemos creado
````
kubectl delete ns argocd testing
````
Ahora ya podemos detener nuestro Minikube
```
minikube stop
```