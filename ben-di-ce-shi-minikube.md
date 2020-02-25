# 使用 MiniKube

為了本地測試kubernetes可在本地安裝 MiniKube

#### 安裝

```
brew install kubectl && brew install minikube
rm /usr/local/bin/kubectl
brew link --overwrite kubernetes-cli
```

> 另外需要安裝virtualBox

#### 執行

```
minikube start --vm-driver=virtualbox
```

之後可以顯示dashboard

```
minikube dashboard
```

## 啟動Pod

```
kubectl apply -f https://k8s.io/examples/application/deployment.yaml
```

[https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/](https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/)

```
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

## 連線到Pod

要先用 `kubectl get pods` 找到 pod 名稱

> 因為 yaml 寫兩個 replicas 所以會有兩個pod![](/assets/螢幕快照 2019-11-09 下午1.28.13.png)

```
kubectl port-forward nginx-deployment-54f57cf6bf-ds6sl 3009:80
```

之後到 localhost:3009 即可看到畫面

## 移除 Pod

```
kubectl delete deployment nginx-deployment
```



