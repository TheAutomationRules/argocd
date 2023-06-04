start: minikubestart installargo project createns gitsecret forward
installiu: installimageupdater installapp sleep forward-app
applicationset1: namespaces1 projectappset1 appset1
applicationset2: namespaces2 projectappset2 repohelm appset2

#----------------------------------------------------------------------------

minikubestart:
	minikube start

installargo:
	@echo "| > INSTALACION DE ARGOCD"
	helm install argo-cd ../Video_02/argo-cd/ \
      --namespace argocd \
      --create-namespace --wait \
      -f ../Video_02/argo-cd/values-custom.yaml

project:
	@echo "| > CREACION DE PROYECTO DEMO"
	kubectl apply -f argocd-manifests/project-argocd-demo.yaml

createns:
	kubectl create ns argocd-demo

gitsecret:
	@echo "| > INSTALACION DE GIT SECRET"
	kubectl apply -f k8s/git-secret.yaml

forward:
	@echo "| > FORWARD DE PUERTO DE ACCESO A ARGOCD"
	kubectl port-forward service/argo-cd-argocd-server -n argocd 8080:443

installimageupdater:
	@echo "| > INSTALACION DE IMAGE UPDATER"
	kubectl apply -f argocd-manifests/image-updater.yaml

installapp:
	@echo "| > INSTALACION DE APLICACION DEMO"
	kubectl apply -f argocd-manifests/nginx-demo-arm.yaml

sleep:
	@echo "DESPLEGANDO..."
	for i in `seq 60 -1 1` ; do echo -ne "segundos para finalizar despliegue! \r$i " ; sleep 1 ; done

forward-app:
	@echo "| > FORWARD DE PUERTO DE ACCESO A APP"
	kubectl port-forward service/nginx-demo-arm -n argocd-demo 8082:80

namespaces1:
	@echo "| > CREACION DE NAMESPACES 1"
	kubectl apply -f k8s/namespaces1.yaml

namespaces2:
	@echo "| > CREACION DE NAMESPACES 2"
	kubectl apply -f k8s/namespaces2.yaml

projectappset1:
	@echo "| > CREACION DE PROJECT APPLICATION SET 1s"
	kubectl apply -f argocd-manifests/project-appset1.yaml

projectappset2:
	@echo "| > CREACION DE PROJECT APPLICATION SET 2"
	kubectl apply -f argocd-manifests/project-appset2.yaml

appset1:
	@echo "| > CREACION DE APPLICATION SET 1"
	kubectl apply -f argocd-manifests/applicationset1.yaml

repohelm:
	@echo "| > CREACION DE REPOSITORY DE HELM"
	kubectl apply -f argocd-manifests/repository.yaml

appset2:
	@echo "| > CREACION DE APPLICATION SET 2"
	kubectl apply -f argocd-manifests/applicationset2.yaml

forward-guestbook:
	@echo "| > FORWARD DE PUERTO DE ACCESO A GUESTBOOK DEV"
	kubectl port-forward service/guestbook-test1-dev-helm-guestbook -n guestbook-dev 8081:80

forward-consul:
	@echo "| > FORWARD DE PUERTO DE ACCESO A CONSUL"
	kubectl port-forward service/stack-consul-dev-consul-server -n stack-dev 8500:8500

forward-prometheus:
	@echo "| > FORWARD DE PUERTO DE ACCESO A VAULT"
	kubectl port-forward service/stack-prometheus-test-server -n stack-test 8082:80

deleteall:
	@echo "| > DELETE ALL APPLICATION SETS"
	kubectl delete applicationset guestbook -n argocd && kubectl delete applicationset stack -n argocd && minikube stop