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
COMPOSER_MEMORY_LIMIT=-1 composer create-project pimcore/demo pimcoredemo
php pimdoredemo/vendor/bin/pimcore-install --mysql-host-socket=mysql --mysql-username=root --mysql-password=root --mysql-database=pimcoredemo --admin-username=admin --admin-password=admin
```