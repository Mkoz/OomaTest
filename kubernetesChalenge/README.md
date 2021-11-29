# Kubernetes Test
Environment:<br>
	Windows 10<br>
	Kubernetes cluseter above docker descktop<br>
	Kubentes dashboard additionally installed<br>

Configuration:<br>
	1 deployment for dashboard "dashboard.yaml"<br>
		modifications:<br>
		add "- --enable-skip-login"<br>
		configured additional container for pcap capture "tcpdumper"<br>
	1 manually created deployment "fe". It provides custom string at http://localhost and pcap capture at http://localhost/tcpdump.html<br>
	1 manually created loadbalancer service "fe_service.yaml" to connect global localhost:30007 outside cluster with pod inside localhost:80.<br>

Note: pcap capture can be easily implemented by starting local "tcpdump cmd", but I choose add additional tcpdum container to separate functionality and show container manipulation skills as requested<br>

To make magick:<br>

@ kubectl.exe apply -f .\fe_dep.yaml -f .\fe_service.yaml -f .\dashboard.yaml<br>
<br>
@ kubectl proxy<br>
<br>
@ kubectl.exe get pod -o wide -A<br>
NAMESPACE              NAME                                        READY   STATUS    RESTARTS   AGE     IP             NODE             NOMINATED NODE   READINESS GATES<br>
default                fe-5d99b57cc7-chb4m                         2/2     Running   0          16m     10.1.0.64      docker-desktop   <none>           <none><br>
kube-system            coredns-558bd4d5db-528x7                    1/1     Running   1          27h     10.1.0.12      docker-desktop   <none>           <none><br>
kube-system            coredns-558bd4d5db-5p7xs                    1/1     Running   1          27h     10.1.0.14      docker-desktop   <none>           <none><br>
kube-system            etcd-docker-desktop                         1/1     Running   1          27h     192.168.65.4   docker-desktop   <none>           <none><br>
kube-system            kube-apiserver-docker-desktop               1/1     Running   1          27h     192.168.65.4   docker-desktop   <none>           <none><br>
kube-system            kube-controller-manager-docker-desktop      1/1     Running   2          27h     192.168.65.4   docker-desktop   <none>           <none><br>
kube-system            kube-proxy-wc87m                            1/1     Running   1          27h     192.168.65.4   docker-desktop   <none>           <none><br>
kube-system            kube-scheduler-docker-desktop               1/1     Running   1          27h     192.168.65.4   docker-desktop   <none>           <none><br>
kube-system            storage-provisioner                         1/1     Running   2          27h     10.1.0.15      docker-desktop   <none>           <none><br>
kube-system            vpnkit-controller                           1/1     Running   40         27h     10.1.0.16      docker-desktop   <none>           <none>
kubernetes-dashboard   dashboard-metrics-scraper-c45b7869d-s6zzc   1/1     Running   1          27h     10.1.0.11      docker-desktop   <none>           <none><br>
kubernetes-dashboard   kubernetes-dashboard-558998898d-tr6w7       2/2     Running   0          5h24m   10.1.0.18      docker-desktop   <none>           <none><br>
<br>
@ kubectl.exe get services -o wide<br>
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE   SELECTOR<br>
fe           LoadBalancer   10.103.79.122   localhost     30007:32007/TCP   80m   app=nginx-test,tier=fe<br>
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP           27h   <none><br>
<br>
@ kubectl.exe get deployments -o wide<br>
NAME   READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS                  IMAGES                              SELECTOR<br>
fe     1/1     1            1           20m   nginx-container,tcpdumper   nginx,docker.io/dockersec/tcpdump   app=nginx-test,tier=fe,track=stable<br>
