# Redis - [Documentation](https://redis.io/documentation)

## [Redis configuration](https://redis.io/topics/config)

### Docker - [Docker Hub](https://hub.docker.com/_/redis)
```
$ docker network create redis
$ docker volume create redis
$ docker run -it --rm --name redis --net redis -v ${PWD}/config/redis-0:/etc/redis/ -v redis:/data/ redis:6.2.6-alpine redis-server /etc/redis/redis.conf
```

## [Security](https://redis.io/topics/security)
By default Redis has not a password, so you need to set the password in redis.conf
```
requirepass PasswordChanged
```

## [Redis Persistence](https://redis.io/topics/persistence)
### RDB (Redis Database)
```
dbfilename dump.rdb
```
### AOF (Append Only File)
```
appendonly yes

appendfilename "appendonly.aof"
```

## [Replication](https://redis.io/topics/replication)
### redis-0
```
dir /data

protected-mode no
port 6379

masterauth MasterPasswordChanged
requirepass PasswordChanged
```
### redis-1
```
dir /data

protected-mode no
port 6379

slaveof redis-0 6379
masterauth MasterPasswordChanged
requirepass PasswordChanged
```
### redis-2
```
dir /data

protected-mode no
port 6379

slaveof redis-0 6379
masterauth MasterPasswordChanged
requirepass PasswordChanged
```
```
$ docker network create redis

$ docker run -d --rm --name redis-0 --net redis \ 
    -v ${PWD}/config/redis-0:/etc/redis/ \ 
    redis:6.2.6-alpine \ 
    redis-server /etc/redis/redis.conf

$ docker run -d --rm --name redis-1 --net redis \ 
    -v ${PWD}/config/redis-1:/etc/redis/ \ 
    redis:6.2.6-alpine \ 
    redis-server /etc/redis/redis.conf

$ docker run -d --rm --name redis-2 --net redis \ 
    -v ${PWD}/config/redis-2:/etc/redis/ \ 
    redis:6.2.6-alpine \ 
    redis-server /etc/redis/redis.conf
```
#### [redis-cli](https://redis.io/topics/rediscli)
```
$ docker exec -it redis-1 sh
$ redis-cli
$ auth "MasterPasswordChanged"
$ keys *
```

## [High Availability - Sentinel](https://redis.io/topics/sentinel)
### sentinel's config files
```
port 5000
sentinel monitor mymaster redis-0 6379 2
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 60000
sentinel parallel-syncs mymaster 1
sentinel auth-pass mymaster a-very-complex-password-here
```
```
$ docker run -d -rm --name sentinel-0 --net redis \
    -v ${PWD}/config/sentinel-0:/etc/redis/ \
    redis:6.2.6-alpine \
    redis-sentinel /etc/redis/sentinel.conf

$ docker run -d -rm --name sentinel-1 --net redis \
    -v ${PWD}/config/sentinel-1:/etc/redis/ \
    redis:6.2.6-alpine \
    redis-sentinel /etc/redis/sentinel.conf

$ docker run -d -rm --name sentinel-2 --net redis \
    -v ${PWD}/config/sentinel-2:/etc/redis/ \
    redis:6.2.6-alpine \
    redis-sentinel /etc/redis/sentinel.conf
```
```
$ docker logs sentinels-0
```
