need docker ce 17.0.3 not 18
setup virtmanager with bridge enabled on host machine

something like this:

auto eno1
iface eno1 inet manual

auto br0
iface br0 inet static
        address 192.168.1.220
        netmask 255.255.255.0
        gateway 192.168.1.1
        metric 0
        bridge_ports eno1
        bridge_stp on
        bridge_fd 0
        bridge_maxwait 0
        dns-nameservers 8.8.8.8 4.4.4.4
        
install docker ce 17.0.3 on single machine then clone to 2 more in VM manager

customize ip config on each machine

auto ens3
iface ens3 inet static
address 192.168.1.232
netmask 255.255.255.0
gateway 192.168.1.1
dns-nameservers 8.8.4.4 8.8.

    

newest version of rke 1.9

do rke config then rke up  - follow the rke instructions and point to the 3 nodes from rke located on the host machine

for kubectl commands - make sure to pass the kubeconfig created when bringing up cluster with rke

kubectl --kubeconfig ~/rke-1.9/kube_config_cluster.yml apply -f install/kubernetes/helm/istio/templates/crds.yaml

FOLLOW INSTRUCTIONS FOR ISTIO INSTALL INTO K8'S (apply commands)

ubuntu@openstack:~/istio/istio-1.0.2$ kubectl --kubeconfig ~/rke-1.9/kube_config_cluster.yml get svc -n istio-system
NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                                                                                                                   AGE
grafana                    ClusterIP      10.43.209.46    <none>        3000/TCP                                                                                                                  23m
istio-citadel              ClusterIP      10.43.204.135   <none>        8060/TCP,9093/TCP                                                                                                         23m
istio-egressgateway        ClusterIP      10.43.227.95    <none>        80/TCP,443/TCP                                                                                                            23m
istio-galley               ClusterIP      10.43.214.25    <none>        443/TCP,9093/TCP                                                                                                          23m
istio-ingressgateway       LoadBalancer   10.43.182.45    <pending>     80:31380/TCP,443:31390/TCP,31400:31400/TCP,15011:30484/TCP,8060:30747/TCP,853:31746/TCP,15030:32362/TCP,15031:30269/TCP   23m
istio-pilot                ClusterIP      10.43.250.219   <none>        15010/TCP,15011/TCP,8080/TCP,9093/TCP                                                                                     23m
istio-policy               ClusterIP      10.43.134.71    <none>        9091/TCP,15004/TCP,9093/TCP                                                                                               23m
istio-sidecar-injector     ClusterIP      10.43.7.56      <none>        443/TCP                                                                                                                   23m
istio-statsd-prom-bridge   ClusterIP      10.43.178.72    <none>        9102/TCP,9125/UDP                                                                                                         23m
istio-telemetry            ClusterIP      10.43.157.125   <none>        9091/TCP,15004/TCP,9093/TCP,42422/TCP                                                                                     23m
jaeger-agent               ClusterIP      None            <none>        5775/UDP,6831/UDP,6832/UDP                                                                                                23m
jaeger-collector           ClusterIP      10.43.232.2     <none>        14267/TCP,14268/TCP                                                                                                       23m
jaeger-query               ClusterIP      10.43.43.210    <none>        16686/TCP                                                                                                                 23m
prometheus                 ClusterIP      10.43.8.105     <none>        9090/TCP                                                                                                                  23m
servicegraph               ClusterIP      10.43.122.240   <none>        8088/TCP                                                                                                                  23m
tracing                    ClusterIP      10.43.92.83     <none>        80/TCP                                                                                                                    23m
zipkin                     ClusterIP      10.43.7.153     <none>        9411/TCP                                                                                                                  23m




ubuntu@openstack:~/istio/istio-1.0.2$ kubectl --kubeconfig ~/rke-1.9/kube_config_cluster.yml get pods -n istio-system
NAME                                        READY     STATUS      RESTARTS   AGE
grafana-85dbf49c94-dpbnz                    1/1       Running     0          2m
istio-citadel-545f49c58b-8xwm9              1/1       Running     0          2m
istio-cleanup-secrets-lk6fb                 0/1       Completed   0          2m
istio-egressgateway-7d59954f4-zhqbn         1/1       Running     0          2m
istio-galley-5b6449c48f-jhknk               1/1       Running     0          2m
istio-grafana-post-install-rk8f7            0/1       Completed   0          2m
istio-ingressgateway-8455c8c6f7-9ml9s       1/1       Running     0          2m
istio-pilot-58ff4d6647-gshzc                2/2       Running     0          2m
istio-policy-59685fd869-2vjgm               2/2       Running     0          2m
istio-security-post-install-kvflf           0/1       Completed   0          2m
istio-sidecar-injector-75b9866679-r9v4g     1/1       Running     0          2m
istio-statsd-prom-bridge-549d687fd9-56t78   1/1       Running     0          2m
istio-telemetry-6ccf9ddb96-sgcbj            2/2       Running     0          2m
istio-tracing-7596597bd7-9vcrv              1/1       Running     0          2m
prometheus-6ffc56584f-rllj9                 1/1       Running     0          2m
servicegraph-5d64b457b4-jzbwq               1/1       Running     0          2m

