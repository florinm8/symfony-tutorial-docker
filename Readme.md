#start dockers
docker-compose up

#enter php container and install symfony web
docker exec -it sym-php bash
symfony new project_name --full
