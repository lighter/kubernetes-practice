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
