#!/bin/env sh

container_name=phpunit-temp
if [[ $(docker ps -a | grep $container_name) ]]; then
    docker rm -f $container_name > /dev/null
fi
docker run --rm --name=$container_name -v $(pwd):/app:rw mkenney/phpunit $@
