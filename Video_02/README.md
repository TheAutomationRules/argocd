# Configuración ⚙️ de Helm Chart de instalación de ArgoCD y Manifiestos 🤘

Hoy veremos como configurar parámetros básicos en el values del Helm Chart de ArgoCD (Ej: contraseña de admin y 
repositorios) y como configurar, repositorios, proyectos y aplicaciones desde manifiestos aplicables desde la CLI.

**Video:**

0:00 Inicio
0:15 Intro
0:27 Descripción
1:48 Configuración e Instalación de ArgoCD desde Values
10:48 Manifiestos y Despliegue
22:18 Eliminación y Desinstalación
23:49 Despedida
23:58 Outro

GitHub:
https://github.com/TheAutomationRules/argocd/blob/main/Video_02/README.md

Twitter: @TheAutomaRules
Instagram: TheAutomationRules

#kubernetes #argocd #gitops #minikube #continuousdelivery

## Instalación con Helm Chart y gestion desde CLI

---
## Requisitos:

- Tener instalado el binario de Helm https://helm.sh/docs/intro/install/
- Tener instalado Minikube https://minikube.sigs.k8s.io/docs/start/
- Tener instalado ArgoCD sigue los pasos en mi video anterior: https://github.com/TheAutomationRules/argocd/blob/main/Video_01/README.md
---
Estando ubicados en el mismo path en el que se encuentra el directorio argo-cd con el chart de ArgoCD descomprimido y vemos los siguientes archivos:
```` 
argo-cd
argo-cd-5.8.2.tgz
````
Duplicamos el archivo values.yaml y creamos una copia con el nombre values-custom.yaml
````
cp argo-cd/values.yaml argo-cd/values-custom.yaml
````
Generamos la contraseña ARGOCDPASS todo en mayusculas con el siguiente comando y la usamos para editar el nuevo values-custom.yaml.
````
htpasswd -nbBC 10 "" ARGOCDPASS | tr -d ':\n' | sed 's/$2y/$2a/'
````
Copiamos el resultado de la contraseña generada sin incluir el caracter de porcentaje del final "%", editamos el values-custom.yaml, buscamos "argocdServerAdminPassword:" y definimos alli la contraseña generada.
````
argocdServerAdminPassword: "$2a$10$sTPh5sJJafZvxb1WG8D6h.seMOewbZKgE/GFTyHcTvgyHF5tboKWm"
````
Buscamos "credentialTemplates:" y aqui definimos el template de las credenciales que usaremos para conectarnos a nuestros repositorios de Git en este caso repositorios de GitHub.
````
github-user-public:
    url: https://github.com/TheAutomationRules
````
Buscamos "repositories: {}" y en esta seccion definimos los repositorios de git que queremos añadir en la instalación de ArgoCD.
````
theautomationrules-argocd:
    url: https://github.com/TheAutomationRules/argocd.git
    name: theautomationrules-argocd
````

Instalamos argocd seleccionando el values file que hemos customizado
````
helm install argo-cd argo-cd/ \
  --namespace argocd \
  --create-namespace --wait \
  -f argo-cd/values-custom.yaml
````
Creamos un namespace de pruebas llamado testing
````
kubectl create ns argocd-demo
````
Ahora podemos crear o añadir el repositorio en el que se encuentra nuestra aplicación a nuestro ArgoCD para que pueda ser usado por el Project y la Application.
````
k apply -f repository.yaml
````
Ahora creamos el Project aplicando el manifest donde hemos hecho la definición del mismo.
````
k apply -f project.yaml
````
Y finalmente creamos la aplicacion para que inicie la sincronización.
````
k apply -f application.yaml
````
Ahora ya podemos ver nuestro repositorio, proyecto y aplicación instaladas, configuradas y sincronizadas!

Si queremos ver la aplicación funcionando hacemos un Port Forward del 8081 al 80 del service
````
kubectl port-forward svc/argocd-demo-helm-guestbook -n argocd-demo 8081:80
````
Accedemos desde el navegador a la dirección localhost:8081 y podemos ver la aplicacion funcionando!

Desinstalamos todo! para ello lo primero, es eliminar la aplicación desde el Dashboard de ArgoCD.

![Captura de pantalla 2023-01-14 a las 12.08.59.png](images%2FCaptura%20de%20pantalla%202023-01-14%20a%20las%2012.08.59.png)

Y ahora desinstalamos el Chart de ArgoCD
````
helm uninstall argo-cd -n argocd
````
Ahora podemos eliminar los Namespaces que hemos creado
````
kubectl delete ns argocd argocd-demo
````
Ahora ya podemos detener nuestro Minikube
```
minikube stop
```