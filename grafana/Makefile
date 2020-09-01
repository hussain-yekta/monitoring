name =  k8s-cluster
namespace = metrics

kubectl := kubectl -n $(namespace)

replacements="\
s|NAMESPACE|$(namespace)|g;\
s|RELEASE-NAME|$(name)|g;\
"
############################################################################################


# Create namespace for Grafana
namespace:
	@cat namespace.yaml | sed $(replacements) | kubectl apply -f -

# Delete namespace for Grafana
delete-namespace:
	@cat namespace.yaml | sed $(replacements) | kubectl delete -f -



# Creating resources for Grafana
run:
	@cat manifests/grafana/templates/clusterrole.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/clusterrolebinding.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/configmap.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/deployment.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/podsecuritypolicy.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/role.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/rolebinding.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/secret.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/service.yaml | sed $(replacements) | kubectl apply -f -
	@cat manifests/grafana/templates/serviceaccount.yaml | sed $(replacements) | kubectl apply -f -


delete:
	@cat namespace.yaml | sed $(replacements) | kubectl delete -f -


# Test the connection 
forward:
	@$(kubectl) port-forward $$($(kubectl) get pod \
		-l "app.kubernetes.io/name=grafana" -o jsonpath="{.items[0].metadata.name}")  3000