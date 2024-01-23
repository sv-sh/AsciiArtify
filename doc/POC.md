ArgoCD interface using k3d:

1.Start
install k3d: Rancher Lab's minimal Kubernetes distribution (runs k3s in Docker)
install kubectl: command-line tool for interacting with the Kubernetes cluste
install ArgoCD CLI: for interacting with ArgoCD

2.Create a Kubernetes Cluster Using k3d

$ k3d cluster create demo
INFO[0000] Prep: Network                                
INFO[0000] Created network 'k3d-demo'                   
INFO[0000] Created image volume k3d-demo-images         
INFO[0000] Starting new tools node...                   
INFO[0000] Starting Node 'k3d-demo-tools'               
INFO[0001] Creating node 'k3d-demo-server-0'            
INFO[0001] Creating LoadBalancer 'k3d-demo-serverlb'    
INFO[0001] Using the k3d-tools node to gather environment information 
INFO[0001] HostIP: using network gateway 172.19.0.1 address 
INFO[0001] Starting cluster 'demo'                      
INFO[0001] Starting servers...                          
INFO[0001] Starting Node 'k3d-demo-server-0'            
INFO[0007] All agents already running.                  
INFO[0007] Starting helpers...                          
INFO[0007] Starting Node 'k3d-demo-serverlb'            
INFO[0014] Injecting records for hostAliases (incl. host.k3d.internal) and for 2 network members into CoreDNS configmap... 
INFO[0016] Cluster 'demo' created successfully!         
INFO[0016] You can now use it like this:                
kubectl cluster-info

$ kubectl cluster-info 
Kubernetes control plane is running at https://0.0.0.0:36511
CoreDNS is running at https://0.0.0.0:36511/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:36511/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

Set Kubeconfig:
$ k3d kubeconfig merge demo --kubeconfig-switch-context
/home/codespace/.config/k3d/kubeconfig-demo.yaml

3.Install ArgoCD
Create ArgoCD Namespace:
$ kubectl create namespace argocd
namespace/argocd created

$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
customresourcedefinition.apiextensions.k8s.io/applications.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/applicationsets.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/appprojects.argoproj.io created
serviceaccount/argocd-application-controller created
serviceaccount/argocd-applicationset-controller created
serviceaccount/argocd-dex-server created
serviceaccount/argocd-notifications-controller created
serviceaccount/argocd-redis created
serviceaccount/argocd-repo-server created
serviceaccount/argocd-server created
role.rbac.authorization.k8s.io/argocd-application-controller created
role.rbac.authorization.k8s.io/argocd-applicationset-controller created
role.rbac.authorization.k8s.io/argocd-dex-server created
role.rbac.authorization.k8s.io/argocd-notifications-controller created
role.rbac.authorization.k8s.io/argocd-server created
clusterrole.rbac.authorization.k8s.io/argocd-application-controller created
clusterrole.rbac.authorization.k8s.io/argocd-server created
rolebinding.rbac.authorization.k8s.io/argocd-application-controller created
rolebinding.rbac.authorization.k8s.io/argocd-applicationset-controller created
rolebinding.rbac.authorization.k8s.io/argocd-dex-server created
rolebinding.rbac.authorization.k8s.io/argocd-notifications-controller created
rolebinding.rbac.authorization.k8s.io/argocd-server created
clusterrolebinding.rbac.authorization.k8s.io/argocd-application-controller created
clusterrolebinding.rbac.authorization.k8s.io/argocd-server created
configmap/argocd-cm created
configmap/argocd-cmd-params-cm created
configmap/argocd-gpg-keys-cm created
configmap/argocd-notifications-cm created
configmap/argocd-rbac-cm created
configmap/argocd-ssh-known-hosts-cm created
configmap/argocd-tls-certs-cm created
secret/argocd-notifications-secret created
secret/argocd-secret created
service/argocd-applicationset-controller created
service/argocd-dex-server created
service/argocd-metrics created
service/argocd-notifications-controller-metrics created
service/argocd-redis created
service/argocd-repo-server created
service/argocd-server created
service/argocd-server-metrics created
deployment.apps/argocd-applicationset-controller created
deployment.apps/argocd-dex-server created
deployment.apps/argocd-notifications-controller created
deployment.apps/argocd-redis created
deployment.apps/argocd-repo-server created
deployment.apps/argocd-server created
statefulset.apps/argocd-application-controller created
networkpolicy.networking.k8s.io/argocd-application-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-applicationset-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-dex-server-network-policy created
networkpolicy.networking.k8s.io/argocd-notifications-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-redis-network-policy created
networkpolicy.networking.k8s.io/argocd-repo-server-network-policy created
networkpolicy.networking.k8s.io/argocd-server-network-policy created

$ kubectl get all -n argocd
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/argocd-redis-b5d6bf5f5-2cw6p                       1/1     Running   0          67s
pod/argocd-notifications-controller-db4f975f8-d2rvs    1/1     Running   0          68s
pod/argocd-applicationset-controller-dc5c4c965-9xfns   1/1     Running   0          68s
pod/argocd-application-controller-0                    1/1     Running   0          66s
pod/argocd-dex-server-9769d6499-25fwm                  1/1     Running   0          68s
pod/argocd-repo-server-579cdc7849-22g66                1/1     Running   0          67s
pod/argocd-server-557c4c6dff-mt5r7                     1/1     Running   0          67s

NAME                                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.43.24.153    <none>        7000/TCP,8080/TCP            68s
service/argocd-dex-server                         ClusterIP   10.43.251.31    <none>        5556/TCP,5557/TCP,5558/TCP   68s
service/argocd-metrics                            ClusterIP   10.43.117.125   <none>        8082/TCP                     68s
service/argocd-notifications-controller-metrics   ClusterIP   10.43.207.150   <none>        9001/TCP                     68s
service/argocd-redis                              ClusterIP   10.43.143.99    <none>        6379/TCP                     68s
service/argocd-repo-server                        ClusterIP   10.43.182.75    <none>        8081/TCP,8084/TCP            68s
service/argocd-server                             ClusterIP   10.43.131.26    <none>        80/TCP,443/TCP               68s
service/argocd-server-metrics                     ClusterIP   10.43.181.122   <none>        8083/TCP                     68s

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-redis                       1/1     1            1           68s
deployment.apps/argocd-notifications-controller    1/1     1            1           68s
deployment.apps/argocd-applicationset-controller   1/1     1            1           68s
deployment.apps/argocd-dex-server                  1/1     1            1           68s
deployment.apps/argocd-repo-server                 1/1     1            1           67s
deployment.apps/argocd-server                      1/1     1            1           67s

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-redis-b5d6bf5f5                       1         1         1       68s
replicaset.apps/argocd-notifications-controller-db4f975f8    1         1         1       68s
replicaset.apps/argocd-applicationset-controller-dc5c4c965   1         1         1       68s
replicaset.apps/argocd-dex-server-9769d6499                  1         1         1       68s
replicaset.apps/argocd-repo-server-579cdc7849                1         1         1       67s
replicaset.apps/argocd-server-557c4c6dff                     1         1         1       67s

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     66s

4. Access the ArgoCD API Server
Expose ArgoCD Service:
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
The tab in browser is opened

![argo](./doc/argo.jpg)

Login Using CLI:
The initial password for the admin account is auto-generated and stored as a secret. Retrieve it using:
$ kubectl get pods -n argocd
 
$ kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d; echo

login use the credentials.


![argo_login](./doc/login_argo.jpg)


