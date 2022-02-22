# 使い方

下記にて、ArgoCDをローカル環境で動作するようにする。
[Getting Started - Argo CD - Declarative GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/getting_started/)

下記より、ArgoCDを使ってローカル環境にNginxをデプロイする。

```shell
$ kubectl apply -k ArgoCDがKustomize_デプロイするアプリケーションがYAML/argocd/overlays -n argocd
application.argoproj.io/nginx created

$ kubectl get svc -n nginx
NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
nginx   ClusterIP   10.107.208.6   <none>        80/TCP    23s

$ kubectl port-forward svc/nginx -n nginx 9090:80
Forwarding from 127.0.0.1:9090 -> 80
Forwarding from [::1]:9090 -> 80
Handling connection for 9090

$ curl -v localhost:9090
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 9090 (#0)
> GET / HTTP/1.1
> Host: localhost:9090
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx/1.21.6
< Date: Tue, 22 Feb 2022 15:23:57 GMT
< Content-Type: text/html
< Content-Length: 615
< Last-Modified: Tue, 25 Jan 2022 15:03:52 GMT
< Connection: keep-alive
< ETag: "61f01158-267"
< Accept-Ranges: bytes
<
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
* Connection #0 to host localhost left intact
* Closing connection 0

$ kubectl delete application/nginx -n argocd
application.argoproj.io "nginx" deleted

$ kubectl delete all --all -n nginx
pod "nginx-cb69f686c-rk6h6" deleted
service "nginx" deleted
deployment.apps "nginx" deleted
replicaset.apps "nginx-cb69f686c" deleted
```
