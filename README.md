# k8s practice

## port forward

```
$ kubectl port-forward  my-pod 8000:3000
```

## create service forward

```
$ minikube status
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100

$ kubectl expose pod my-pod --type=NodePort --name=my-pod-service
$ kubectl get services

# get url
$ minikube service my-pod-service --url
http://192.168.99.100:32300
```


## 常見指令

```
$ kubectl get pods
$ kubectl get pods --show-all
$ kubectl describe pod <POD>

# 指定 port number expose 出讓外部服務存取（建立一個新的 Service 物件）
$ kubectl expose pod <POD> --port=<PORT> --name=<SERVICE-NAME>

# 指定 port number mapping 到本機某一特定的 port
$ kubectl port-forward <POD> <EXTERNAL-PORT>:<POD-PORT>

# 進到 container 看內部的 logs
$ kubectl attach <POD> -i

# 針對 pod 內部下指令
$ kubectl exec <POD> -- <COMMADN>
$ kubectl exec my-pod -- ls /app

# 顯示目前的 labels
$ kubectl get pod --show-labels

# 新增 pod 的 labels
$ kubectl label pods <POD> <LABEL-KEY>=<LABEL-VALUE>
```


## Replication controller

指定同時有多少個相同的 pods 運作

```
# 取得目前 replication controller
$ kubectl get rc

# 增加 pod 的數量
$ kubectl scale --replicas=<NUMBER> -f <REPLICATION-CONTROLLER.yaml>

# 刪除 replication controller，原有的 pod 還可運作
$ kubectl delete rc my-replication-controller --cascade=false

# 強迫跟著刪除 pod
$ kubectl delete pods <POD> --grace-period=0 --force
```


## Deployment 常用相關指令

| Deployment 指令                                                               | 解說                                        |
| ----------------------------------------------------------------------------- | ------------------------------------------ |
| kubectl get deployments                                                       | 取得目前 k8s 的 deployments 資訊             |
| kubectl get rs                                                                | 取得目前 k8s 的 replication set 資訊         |
| kubectl describe deploy <DEPLOYMENT-NAME>                                     | 取得指定 deployment 資訊                     |
| kubectl set image deploy/ <DEPLOYMENT-NAME> <POD-NAME>:<IMAGE-PATH>:<VERSION> | 將 deployment 管理的 pod 升級到特定 image 版本 |
| kubectl edit deployment <DEPLOYMENT-NAME>                                     | 編輯特定 deployment                          |
| kubectl rollout status deploy <DEPLOYMENT-NAME>                               | 查看特定 deployment 升級狀態                  |
| kubectl rollout history deploy <DEPLOYMENT-NAME>                              | 查看特定 deployment 升級歷史紀錄               |
| kubectl rollout undo deploy <DEPLOYMENT-NAME>                                 | 返回上一個版本 pod                            |
| kubectl rollout undo deploy <DEPLOYMENT-NAME>  --to-revision=<N>              | 返回到指定版本 pod                            |

```
# deployment service
$ kubectl expose deploy <DEPLOYMENT-NAME> --type=NodePort --name=<DEPLOYMENT-SERVICE-NAME>

# rollout docker image
$ kubectl set image deploy/<DEPLOYMENT-NAME> <POD-NAME>=<IMAGE-NAME>:<VERESION-TAG>

# 查看 rollout status
$ kubectl rollout status deploy <DEPLOYMENT-NAME>

# 編輯 deployment
$ kubectl edit deploy <DEPLOYMENT-NAME>

# 查看 deploy rollout history
$ kubectl rollout history deploy <DEPLOYMENT-NAME>

# 紀錄每次 rollout 
$ kubectl set image deploy/<DEPLOYMENT-NAME> <POD-NAME>=<IMAGE-NAME>:<VERSION-TAG> --record

# 返回上一版 deployment
$ kubectl rollout undo deploy <DEPLOYMENT-NAME>

# 指定回到某個版本
# N 可用 history 查看第幾個
$ kubectl rollout undo deploy <DEPLOYMENT-NAME> --to-revision=<N>
```