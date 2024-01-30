# Como anidar 🪺 Aplicaciones en ArgoCD 🐙

Hoy vamos a ver como anidar aplicaciones en ArgoCD para organizar el despliegue de nuestras aplicaciones en nuestro clúster de Kubernetes permitiéndoos tener en cada una de ellas configuraciones distintas en cuanto al comportamiento de la sincronización de cada una de ellas.

**Video:** https://youtu.be/EQz7pB7I4bQ

0:00 Inicio

0:05 INTRO

0:17 Descripción

0:35 Instalación de ArgoCD

3:45 Project argocd-demo

4:31 Application argocd-demo

5:33 Demo Helm Chart Apps

13:23 Conclusión

14:44 Despedida

14:53 OUTRO

GitHub:
https://github.com/TheAutomationRules/argocd/blob/main/Video_05/README.md

Twitter: @TheAutomaRules
Instagram: TheAutomationRules

#kubernetes #argocd #application #gitops #continuousdelivery #helm #chart

---

## Instrucciones:

Instalamos ArgoCD basado en el Helm Chart en su ultima version estable 5.51.6 haciendo uso del Makefile que tenemos en el repositorio de codigo, lo hacemos parados desde el path del Video_05.
````
make installargo
````
Como estamos usando el values-custom.yaml que tenemos personalizado con lo minimo que hemos usado en otros vídeos, levantamos un port forward para acceder a nuestro ArgoCD desde el navegador a la dirección localhost:8080.
````
kubectl port-forward service/argo-cd-argocd-server -n argocd 8080:443
````
Creamos o mas bien aplicamos el manifiesto para la creacion de un project llamado argocd-demo.
````
make project
````
Aplicamos el manifiesto de la aplicacion base o aplicacion inicial que nos sirve de launcher para desencadenar la creacion del resto de aplicaciones.
````
make application
````
Sincronizamos por fases los elementos de la aplicacion base desde ArgoCD, continuamos con el contenido del video.

**The Automation Rules** 🤖👍