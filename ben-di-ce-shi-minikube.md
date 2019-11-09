# 使用 MiniKube

為了本地測試kubernetes可在本地安裝 MiniKube

#### 安裝

```
brew install kubectl Minikube
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



