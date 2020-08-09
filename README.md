# nginx-deployment

2) create nginx-deployment file

[root@Master kube]# kubectl create -f nginx_deploy.yaml

deployment.apps/nginx-deployment created
[root@Master kube]#

---------------------------------------------------------------------------------

[root@Master kube]# kubectl get po -o wide


NAME                                READY   STATUS    RESTARTS   AGE   IP          NODE    NOMINATED NODE   READINESS GATES
nginx-deployment-57796d7dff-h9rr4   1/1     Running   0          34s   10.10.2.6   node2   <none>           <none>
nginx-deployment-57796d7dff-rx7jf   1/1     Running   0          34s   10.10.1.6   node1   <none>           <none>
[root@Master kube]#

[root@Master kube]# kubectl get deploy


NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           101s
[root@Master kube]#

---------------------------------------------------------------------------------
3) update a deployment

[root@Master kube]# kubectl set image deployment nginx-deployment nginx=nginx:1.16.1 --record

deployment.apps/nginx-deployment image updated

[root@Master kube]#
----------------------------------------------------------------------------------------------------------------------------------

[root@Master kube]# kubectl rollout status deployment.v1.apps/nginx-deployment
deployment "nginx-deployment" successfully rolled out

[root@Master kube]#

-----------------------------------------------------------------------------------------------------------------------------------
[root@Master kube]# kubectl rollout history deployment.v1.apps/nginx-deployment
deployment.apps/nginx-deployment

REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx-deployment nginx=nginx:1.16.1 --record=true

[root@Master kube]#

---------------------------------------------------------------------------------------------------------------------------------------
#[root@Master kube]# kubectl describe deployment nginx-deployment

Containers:
   nginx:
    Image:        nginx:1.16.1

-------------------------------------------------------------------------------------------------------------------------------------

[root@Master kube]# kubectl get all -o wide


NAME                                    READY   STATUS    RESTARTS   AGE     IP          NODE    NOMINATED NODE   READINESS GATES
pod/nginx-deployment-5bc5c6fcb4-6xt57   1/1     Running   0          29s     10.10.2.8   node2   <none>           <none>
pod/nginx-deployment-5bc5c6fcb4-9htjb   1/1     Running   0          6m56s   10.10.1.7   node1   <none>           <none>
pod/nginx-deployment-5bc5c6fcb4-qfpst   1/1     Running   0          6m54s   10.10.2.7   node2   <none>           <none>

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE   SELECTOR
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   12m   <none>

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-deployment   3/3     3            3           11m   nginx        nginx:1.16.1   app=nginx

NAME                                          DESIRED   CURRENT   READY   AGE     CONTAINERS   IMAGES         SELECTOR
replicaset.apps/nginx-deployment-57796d7dff   0         0         0       11m     nginx        nginx:1.14.2   app=nginx,pod-template-hash=57796d7dff
replicaset.apps/nginx-deployment-5bc5c6fcb4   3         3         3       6m56s   nginx        nginx:1.16.1   app=nginx,pod-template-hash=5bc5c6fcb4
[root@Master kube]#

-------------------------------------------------------------------------------------------------------------------------------------------------

[root@Master kube]# kubectl get deploy

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           12m
[root@Master kube]#

-------------------------------------------------------------------------------------------------------------------------------------------

[root@Master kube]# kubectl rollout undo deployment.v1.apps/nginx-deployment

deployment.apps/nginx-deployment rolled back

[root@Master kube]#

----------------------------------------------------------------------------------------------------------------------------------------------

#kubectl describe po nginx-deployment

Containers:
  nginx:
    Container ID:   docker://09b49b9b7612c3d5f4ee9fd42555bd008329053f92b0a96c958e531ee77a44c0
    Image:          nginx:1.14.2
