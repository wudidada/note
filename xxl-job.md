# xxl-job部署

## 启动数据库并执行初始化脚本

```
docker run -d --network xxl-job --network-alias mysql -v xxl-job-mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root --name mysql mysql:5.7
wget https://raw.githubusercontent.com/xuxueli/xxl-job/master/doc/db/tables_xxl_job.sql
docker exec -i mysql mysql -u root -proot < tables_xxl_job.sql
```

## 启动xxl-job-admin

```
docker run -dp 8080:8080 --network xxl-job --network-alias xxl-job-admin --name xxl-job-admin -v /tmp:/data/applogs -e PARAMS="--spring.datasource.url=jdbc:mysql://mysql:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai --spring.datasource.username=root  --spring.datasource.password=root" xuxueli/xxl-job-admin:2.3.0
```

## 启动xxl-job-executor

```
docker run --network xxl-job -v /tmp:/data/applogs -e PARAMS="--xxl.job.admin.addresses=http://xxl-job-admin:8080/xxl-job-admin" xxl-job-executor
```

