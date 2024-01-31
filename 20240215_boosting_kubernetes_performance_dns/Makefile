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

download-node-local-dns-cache-manifest:
	@curl -o manifests/nodelocaldns.yaml https://raw.githubusercontent.com/kubernetes/kubernetes/master/cluster/addons/dns/nodelocaldns/nodelocaldns.yaml

# see https://kubernetes.io/docs/tasks/administer-cluster/nodelocaldns/#configuration
patch-node-local-dns-cache-manifest:
	$(eval KUBEDNS=$(shell kubectl get svc kube-dns -n kube-system -o jsonpath={.spec.clusterIP}))
	$(shell sed -i '' "s/__PILLAR__LOCAL__DNS__/169.254.20.10/g; s/__PILLAR__DNS__DOMAIN__/cluster.local/g; s/__PILLAR__DNS__SERVER__/$(KUBEDNS)/g" manifests/nodelocaldns.yaml)

deploy-node-local-dns-cache: download-node-local-dns-cache-manifest patch-node-local-dns-cache-manifest
	@kubectl apply -f manifests/nodelocaldns.yaml

delete-node-local-dns-cache:
	@kubectl delete -f manifests/nodelocaldns.yaml

patch-coredns-configmap:
	@kubectl patch configmap/coredns \
 		-n kube-system \
  		--type merge \
  		--patch-file manifests/coredns-patch.yaml
	
configure-coredns: patch-coredns-configmap
	@kubectl rollout restart deployment coredns -n kube-system

.PHONY: deploy-kube-prometheus-stack grafana-port-forwarding deploy-resources start-shell-first start-shell-second follow-coredns-logs deploy-node-local-dns-cache delete-node-local-dns-cache configure-coredns