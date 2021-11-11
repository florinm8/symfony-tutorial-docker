###start containers
docker-compose up

###enter php container and install symfony web
```bash
docker exec -it sym-php bash
```

```bash
symfony new project_name --full
```

Optional install pimcore demo app  
```bash
echo 'create database pimcoredemo;' | docker exec -i sym-db mysql -uroot -proot
```

```bash
COMPOSER_MEMORY_LIMIT=-1 composer create-project pimcore/demo pimcoredemo
php pimcoredemo/vendor/bin/pimcore-install --mysql-host-socket=mysql --mysql-username=root --mysql-password=root --mysql-database=pimcoredemo --admin-username=admin --admin-password=admin
```