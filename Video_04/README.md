# Como crear ApplicationSets 📦en ArgoCD 🐙

Hoy os voy a mostrar como crear un set de aplicaciones o ApplicationSet en ArgoCD consiguiendo así estandarizar el despliegue de varias aplicaciones o servicios.

**Video:**

0:00 Inicio
0:08 INTRO
0:19 Minikube y Makfile
1:59 Acceso a ArgoCD
3:55 DEMO applicationset1
10:05 DEMO applicationset2
14:55 DEMO acceso a aplicaciones
17:52 Despedida
18:01 OUTRO

GitHub:
https://github.com/TheAutomationRules/argocd/blob/main/Video_04/README.md

Twitter: @TheAutomaRules
Instagram: TheAutomationRules

#kubernetes #argocd #applicationset #gitops #minikube #continuousdelivery

---

## Instalación basado en el video 02 instalando directamente desde el Makefile

Instalamos ArgoCD
````
make start
````
Desplegamos los namespaces, project y application set del primer ejemplo
````
make applicationset1
````
Con esto comprobamos en ArgoCD como se despliegan las aplicaciones definidas en el ApplicationSet 1.

Ahora podemos por ver el Guest Book en funcionamiento, levantamos un forward al de Dev

````
make forward-guestbook
````
Aqui podemos ver la aplicacion funcionando y podemos hacer uso de ella.
````
make applicationset2
````
Con esto comprobamos en ArgoCD como se despliegan las aplicaciones definidas en el ApplicationSet 2.

Ahora podemos ver las aplicaciones funcionando, levantamos un forward para ver Consul funcionando.
````
make forward-consul
````
Levantamos un forward para ver Prometheus funcionando.
````
make forward-prometheus
````
Si queremos eliminar todos los application sets
````
make deleteall
````