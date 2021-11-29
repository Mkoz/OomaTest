# Helm Test

>>> helm version<br>
version.BuildInfo{Version:"v3.7.1", GitCommit:"1d11fcb5d3f3bf00dbe6fe31b8412839a96b3dc4", GitTreeState:"clean", GoVersion:"go1.16.9"}<br>
<br>
>>> helm repo add bitnami https://charts.bitnami.com/bitnami<br>
>>> helm repo update<br>
<br>
>>> helm search repo nginx<br>
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION<br>
bitnami/nginx                           9.5.14          1.21.4          Chart for the nginx server<br>
bitnami/nginx-ingress-controller        9.0.8           1.1.0           Chart for the nginx Ingress controller<br>
bitnami/kong                            4.1.9           2.6.0           Kong is a scalable, open source API layer (aka ...<br>
<br>
>>> kubectl create namespace nginx<br>
<br>
>>> helm install nginx bitnami/nginx -n nginx --version=9.5.13<br>
NAME: nginx<br>
LAST DEPLOYED: Mon Nov 29 10:47:38 2021<br>
NAMESPACE: nginx<br>
STATUS: deployed<br>
REVISION: 1<br>
TEST SUITE: None<br>
NOTES:<br>
CHART NAME: nginx<br>
CHART VERSION: 9.5.13<br>
APP VERSION: 1.21.4<br>
<br>
** Please be patient while the chart is being deployed **<br>
<br>
NGINX can be accessed through the following DNS name from within your cluster:<br>
<br>
    nginx.nginx.svc.cluster.local (port 80)<br>
<br>
To access NGINX from outside the cluster, follow the steps below:<br>
<br>
1. Get the NGINX URL by running these commands:<br>
<br>
  NOTE: It may take a few minutes for the LoadBalancer IP to be available.<br>
        Watch the status with: 'kubectl get svc --namespace nginx -w nginx'<br>
<br>
    export SERVICE_PORT=$(kubectl get --namespace nginx -o jsonpath="{.spec.ports[0].port}" services nginx)<br>
    export SERVICE_IP=$(kubectl get svc --namespace nginx nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')<br>
    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
<br>
<br>
>>> kubectl.exe get svc -n nginx -o=wide<br>
NAME    TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE   SELECTOR<br>
nginx   LoadBalancer   10.96.59.18   localhost     80:32098/TCP   91s   app.kubernetes.io/instance=nginx,app.kubernetes.io/name=nginx<br>
<br>
>>> helm upgrade nginx bitnami/nginx -n nginx --version=9.5.13 --set service.type="NodePort"<br>
<br>
>>> kubectl.exe get svc -n nginx -o=wide<br>
NAME    TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE   SELECTOR<br>
nginx   NodePort   10.96.59.18   <none>        80:32098/TCP   10m   app.kubernetes.io/instance=nginx,app.kubernetes.io/name=nginx<br>