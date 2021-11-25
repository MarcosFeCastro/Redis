# kubernetes

## kind cluster
```
$ kind create cluster --name redis
```

## Namespace
```
$ kubectl create ns redis
```

## Storage Class
```
$ kubectl apply -n redis -f redis-local-storage.yaml
```
```
$ kubectl get storageclass
```

## ConfigMap Deploy
```
dir /data

dbfilename dump.rdb

appendonly yes
appendfilename "appendonly.aof"

masterauth password-changed
requirepass password-changed
```
```
bind 127.0.0.1
```
or
```
bind 0.0.0.0
```

```
$ kubectl apply -n redis -f redis-configmap.yaml
```

## Redis Nodes
```
$ kubectl apply -n redis -f redis-statefulset.yaml
```
```
$ kubectl -n redis get pods
$ kubectl -n redis get pv
```
```
$ kubectl -n redis logs redis-0
$ kubectl -n redis logs redis-1
$ kubectl -n redis logs redis-2
```
```
$ kubectl exec -it -n redis redis-0 sh
$ redis-cli
$ auth "password-changed"
$ info replication
```

## Setinels Nodes
```
$ kubectl apply -n redis -f sentinel-statefulset.yaml
```
```
$ kubectl -n redis get pods -o wide
```
```
$ kubectl -n redis logs sentinel-0
$ kubectl -n redis logs sentinel-1
$ kubectl -n redis logs sentinel-2
```