start: minikubestart installargo project createns gitsecret forward
installiu: installimageupdater installapp sleep forward-app

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