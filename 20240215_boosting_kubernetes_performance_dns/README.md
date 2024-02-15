# Boosting Kubernetes Performance - How to tune DNS resolution

contact: f.brundke@reply.de

This talk was held at [Kubernetes and Cloud Native Meetup](https://www.meetup.com/de-DE/munchen-kubernetes-meetup/) in Munich on Feb 15th 2024. You can find the slides and accompanying material in this repo folder.

For the demo part I used a [Colima](https://github.com/abiosoft/colima) cluster. You should be able to use any cluster that has CoreDNS installed. Follow the steps below to configure the environment like in the demo (if you are not on a Mac you might need to adjust some commends in the Makefile):


1. Start e.g. Colima cluster: `colima start --kubernetes`
1. Once the cluster is ready patch the coredns configmap to enable logging (and remove noisy warnings): `make configure-coredns`
1. Deploy Kubernetes resources: `make deploy-resources` (this will deploy two namespaces, each with an nginx pod + service and a pod used as shell)
1. To deploy [NodeLocal DNSCache](https://kubernetes.io/docs/tasks/administer-cluster/nodelocaldns/) run: `make deploy-node-local-dns-cache`
1. Optional (if you want to check CoreDNS metrics):
    1. Install [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) (if you want you can adjust the values to not install unneccessary functionality like alertmanager): `make deploy-kube-prometheus-stack`
    1. Start port forwarding for Grafana: `make grafana-port-forwarding`
    1. Open Grafana dashboard in a browser on `127.0.0.1:3000` and log in with default credentials (admin/prom-operator)
    1. Create a new dashboard for CoreDNS by importing https://grafana.com/grafana/dashboards/15762-kubernetes-system-coredns/ and check metrics

