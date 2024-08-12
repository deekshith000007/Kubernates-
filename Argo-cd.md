##setup argocd on microk8s 

argocd steps:
1.

```sh 
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
```

2. Expose the service
```sh 
kubectl get svc -n argocd
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
3.Tunnelining use 
```sh 
kubectl get svc argocd-server -n argocd
```

just to check :-->
```sh 
kubectl get svc argocd-server -n argocd --watch
```
4.
```sh 
root@ip-172-31-86-153:~kubectl describe svc argocd-server -n argocdocd
Name:                     argocd-server
Namespace:                argocd
Labels:                   app.kubernetes.io/component=server
                          app.kubernetes.io/name=argocd-server
                          app.kubernetes.io/part-of=argocd
Annotations:              <none>
Selector:                 app.kubernetes.io/name=argocd-server
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.152.183.199
IPs:                      10.152.183.199
Port:                     http  80/TCP
TargetPort:               8080/TCP
NodePort:                 http  31581/TCP
Endpoints:                10.1.142.72:8080
Port:                     https  443/TCP
TargetPort:               8080/TCP
NodePort:                 https  31431/TCP
Endpoints:                10.1.142.72:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

5. Enableing the ingress:
```sh 
microk8s enable ingress
```
```sh 
root@ip-172-31-86-153:~# kubectl get svc argocd-server -n argocd
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-server   LoadBalancer   10.152.183.199   <pending>     80:31581/TCP,443:31431/TCP   9m20s
```
6. To access the application 
```sh 
 <public ip>:31581
```

7. TO get the secrete key

```sh
kubectl get secret <argocd-secret-name> -n argocd -o jsonpath='{.data.admin\.password}' | base64 --decode
```




