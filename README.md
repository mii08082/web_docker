## Docker　Laravel環境

```
$ docker-compose ps
        Name                       Command               State                 Ports
--------------------------------------------------------------------------------------------------
web_docker_app_1        docker-php-entrypoint php-fpm    Up      9000/tcp
web_docker_database_1   docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp, 33060/tcp
web_docker_kvs_1        docker-entrypoint.sh redis ...   Up      6379/tcp
web_docker_web_1        /docker-entrypoint.sh ngin ...   Up      0.0.0.0:80->80/tcp
```
## databaseログイン
docker-compose exec database bash -c 'mysql -uroot -ppassword'

## appログイン
docker-compose exec app ash



## アクセス
http://localhost:10088/


