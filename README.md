# consul-k8s-prometheus-grafana-hashicups-demoapp
This repo contains application and dashboard definitions for the Consul Layer 7 observability with Kubernetes.

To fully deploy the app run the following scripts in order. Assumes you have a Kubernetes cluster available.  Tested with Minikube and Kind.

`helm install -f helm/consul-values.yaml consul hashicorp/consul --version "0.23.1" --wait`

`helm install -f helm/prometheus-values.yaml prometheus prometheus-community/prometheus --version "11.7.0" --wait`

`helm install -f helm/grafana-values.yaml grafana grafana/grafana --version "5.3.6" --wait`

`kubectl apply -f app`

To simulate a load on the applicaton, run `kubectl apply -f traffic.yaml`.

You can then monitor the different applications from within a web browser with the following url's


## Grafana
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace default port-forward $POD_NAME 3000

Open a web brpwser to http://localhost:3000


## Prometheus
kubectl port-forward deploy/prometheus-server 9090:9090

Open a web brpwser to http://localhost:9090


## Consul
kubectl port-forward consul-server-0 8500:8500

Open a web brpwser to http://localhost:8500
