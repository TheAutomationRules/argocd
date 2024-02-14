argoenv: installargo project application

installargo:
	@echo "| > INSTALACION DE ARGOCD"
	helm install argo-cd argo/argo-cd --version 5.51.6 \
      --namespace argocd \
      --create-namespace --wait \
      -f values-custom.yaml

project:
	@echo "| > CREACION DE PROYECTO DEMO"
	kubectl apply -f argocd-manifests/project-argocd-demo.yaml

application:
	@echo "| > CREACION DE APLICACION DEMO"
	kubectl apply -f argocd-manifests/application.yaml
