# Boosting Kubernetes Performance - How to tune DNS resolution
Slides can be found here ...

For the demo part I used a minkube cluster. You should be able to use any cluster that has CoreDNS installed. 

## Configure environment

1. Start e.g. minikube with 3 nodes: `minikube start --driver qemu --network socket_vmnet --nodes 3 --profile 3nc`
1. Deploy Kubernetes resources: `make deploy-resources` (this will deploy two namespaces, each with an nginx pod + service and a pod used as shell)
1. Optional:
    1. Install [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) (if you want you can adjust the values to not install unneccessary functionality like alertmanager): `make deploy-kube-prometheus-stack`
    1. Start port forwarding for Grafana: `make grafana-port-forwarding`
    1. Open Grafana dashboard in a browser on `127.0.0.1:3000`` and log in with default credentials (admin/prom-operator)
    1. Create a new dashboard for CoreDNS by importing https://grafana.com/grafana/dashboards/15762-kubernetes-system-coredns/ and check metrics

