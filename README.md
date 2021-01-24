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

## migrate実行
docker-compose exec app php blog/artisan migrate
docker-compose exec --user 1000 app php blog/artisan make:migration create_user_profile_table3

## Controller作成
docker-compose exec  --user 1000 app php blog/artisan make:controller RegistrationController

## アクセス
http://localhost:10088/

## 対処：laravel SQLSTATE[HY000] [2002] Connection refused
docker-compose exec app php blog/artisan migrate実行時下記エラーが出た場合
```
 SQLSTATE[HY000] [2002] Connection refused (SQL: select * from information_schema.tables where table_schema = laravel and table_name = migrations and table_type = 'BASE TABLE')

  at blog/vendor/laravel/framework/src/Illuminate/Database/Connection.php:678
    674▕         // If an exception occurs when attempting to run a query, we'll format the error
    675▕         // message to include the bindings with SQL, which will make this exception a
    676▕         // lot more helpful to the developer instead of just the database's errors.
    677▕         catch (Exception $e) {
  ➜ 678▕             throw new QueryException(
    679▕                 $query, $this->prepareBindings($bindings), $e
    680▕             );
    681▕         }
    682▕
```
下記コマンド実行する
```
docker-compose exec app php blog/artisan key:generate
docker-compose exec app php blog/artisan cache:clear
docker-compose exec app php blog/artisan route:clear
docker-compose exec app php blog/artisan config:clear
docker-compose exec app php blog/artisan view:clear
docker-compose exec app php blog/artisan migrate
```
このようになれば成功
```
$ docker-compose exec app php blog/artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table (62.07ms)
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table (50.47ms)
Migrating: 2019_08_19_000000_create_failed_jobs_table
Migrated:  2019_08_19_000000_create_failed_jobs_table (41.75ms)
Migrating: 2021_01_24_080938_create_items_table
Migrated:  2021_01_24_080938_create_items_table (19.35ms)
```