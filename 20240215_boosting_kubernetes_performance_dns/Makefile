deploy-kube-prometheus-stack:
	@helm install kube-prometheus-stack --create-namespace -n kube-prometheus-stack prometheus-community/kube-prometheus-stack

grafana-port-forwarding:
	@kubectl port-forward deployment/kube-prometheus-stack-grafana -n kube-prometheus-stack 3000

deploy-resources:
	@kubectl apply -f manifests/setup-ns-first.yaml -f manifests/setup-ns-second.yaml

start-shell-first:
	@kubectl exec shell -n first -it -- sh
	
start-shell-second:
	@kubectl exec shell -n second -it -- sh

follow-coredns-logs:
	@kubectl logs deployment/coredns -n kube-system -f

.PHONY: deploy-kube-prometheus-stack grafana-port-forwarding deploy-resources start-shell-first start-shell-second follow-coredns-logs