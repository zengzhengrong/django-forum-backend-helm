### django-forum-backend-helm
helm chart for django-forum-backend


### Install 


#### Config charts/django-forum-backend/values.yaml and install all resources by
```
helm install django-forum-backend charts/django-forum-backend/ --namespace default --values charts/django-forum-backend/values.yaml
```

#### Just enable nginx with api ,config your redis and db

```
helm install django-forum-backend charts/django-forum-backend/ --namespace default \
--set mysql.enabled=false \
--set redis.enabled=false \
--set phpmyadmin.enabled=false \
--set api.configMap.databaseUrl=mysql://{{user}}:{{user-passwrod}}@{{host}}:{{host-port}}/{{dbname}} \
--set api.configMap.redisUrl=redis://{{host}}:{{port}} \
--set api.deploy.containers.command.waitDB={{host}} \
--values charts/django-forum-backend/values.yaml
```




#### If enable ingress by 

```
helm install django-forum-backend charts/django-forum-backend/ --namespace default \
--set nginx.ingress.enabled=true \
--set nginx.ingress.hosts[0].host=xxx.com \
--set phpmyadmin.ingress.enabled=true \
--set phpmyadmin.ingress.hosts[0].host=xxx.com \
--values charts/django-forum-backend/values.yaml
```


#### Config pvc by 

api,  required
```
--set api.persistentVolumeClaim.accessModes=ReadWriteMany
--set api.persistentVolumeClaim.storageClassName=nfs-client
--set api.persistentVolumeClaim.storage=100M
```

mysql, if enabled

```
--set mysql.persistentVolumeClaim.accessModes=ReadWriteMany
--set mysql.persistentVolumeClaim.storageClassName=nfs-client
--set mysql.persistentVolumeClaim.storage=100M
```

redis, if enabled

```
--set redis.persistentVolumeClaim.accessModes=ReadWriteMany
--set redis.persistentVolumeClaim.storageClassName=nfs-client
--set redis.persistentVolumeClaim.storage=100M
```

### Show all config

```
helm show values django-forum-backend/
```

### Upgrade
If install by 

```
helm install django-forum-backend charts/django-forum-backend/ --namespace default \
--set mysql.enabled=false \
--set redis.enabled=false \
--set phpmyadmin.enabled=false \
--set api.configMap.databaseUrl=mysql://{{user}}:{{user-passwrod}}@{{host}}:{{host-port}}/{{dbname}} \
--set api.configMap.redisUrl=redis://{{host}}:{{port}} \
--set api.deploy.containers.command.waitDB={{host}} \
--values charts/django-forum-backend/values.yaml
```

And want to enable ingress by

```
helm upgrade django-forum-backend charts/django-forum-backend/ \
--namespace default  \
--reuse-values \
--set nginx.ingress.enabled=true \
--set nginx.ingress.hosts[0].paths[0]=/ \
--set nginx.ingress.hosts[0].host=xxx.com

```