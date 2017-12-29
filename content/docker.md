Title: Introducción a Docker
Date: 2017-12-29
Modified: 2017-12-29
Category: Docker
Tags: Docker, Deployment
Authors: Santiago Blanco
Summary: Resumen de los mandatos más usados en Docker.

# Docker

## Ejecutar un contenedor

Docker puede ejecutar contenedores de diferentes maneras:
* **Single task:** Esto es ejecutar una tarea y terminar.
* **Interactive:** Usar el contenedor de forma interactiva, algo así como SSH.
* **Background:** Usarlo como demonio.

### Single task

Para ejecutar un mandato en un contenedor:

    docker container run alpine hostname

Para listar todos los contenedores existentes:

    docker container ls --all

### Interactivo

Para usar de manera interactiva un programa es necesario ejecutar algo parecido al siguiente mandato:

    docker container run --interactive --tty --rm ubuntu bash

donde:
* --rm elimina el contenedor cuando termina su ejecución.
* --interactive hace que el contenedor se ejecute en modo interactivo.

### Background

Para ejecutar un contenedor en modo background hay que escribir algo parecido a lo siguiente:

    docker container run --detach --name mydb -e MYSQL_ROOT_PASS=1234 mysql:latest

donde:
* --detach hace que sea un contenedor en modo background.
* --name especifica el nombre del contenedor que se quiere usar.
* -e MYSQL_ROOT_PASS=1234 establece dentro del contenedor una variable de entorno.

Para ver los logs generados por la aplicación podemos ejecutar:

    docker logs mydb

Para ejecutar un mandato en un contenedor que está corriendo en este modo:

    docker exec -it mydb sh

## Crear una imagen

Para crear una imagen de docker hay que crear un Dockerfile. Un ejemplo bastante básico de Dockerfile puede ser el siguiente:

    FROM nginx:latest
    COPY index.html /usr/share/nginx/html
    EXPOSE 80 443
    CMD [“nginx”, “-g”, “daemon off;”]

Con este fichero podríamos crear una imagen basada en otra imagen denominada nginx, se copia un fichero del host en la imagen, se abren los puertos 80 y 443, y se ejecuta nginx al iniciar el contenedor.

Para crear la imagen de los contenedores tan sólo habría que ejecutar, en el directorio donde se encuentre el fichero Dockerfile, el siguiente mandato:

    docker image build --tag yomismo/mynginx:1.0 .

donde:
* --tag <user_name/image_name:version> es el identificador que se le asigna a la imagen.
* el punto final es el path donde se encuentra el Dockerfile

## Ejecución de contenedores

Después de crear la imagen, ya se puede ejecutar el contenedor (la imágen está en el registro de docker local de nuestra máquina).

Como el contenedor creado es un servidor HTTP, debemos ejecutar el contenedor en modo Background:

    docker container run -d --publish 80:80 --name mynginx yomismo/mynginx:1.0

donde:
* -d es equivalente a --detach, es decir, para ejecutar en modo background.
* --publish <host:container> crea un tunel entre el puerto del host y el del contenedor.
* --name <nombre> le asigna un identificador al contenedor.

Si quisiéramos probar una aplicación que estamos ejecutando actualmente, tan solo tendríamos que agregar la opción mount apropiadamente:

    docker container run --mount=bind,source=”host_path”,target=”container_path”

donde:
* --mount con la opción “bind” provoca que cada cambio que realicemos en nuestro en el directorio del host se va a ver reflejado en el contenedor.

Cuando hayamos terminado de ejecutar el contenedor deberíamos pararlo y borrarlo:

    docker container rm --force mynginx

donde:
* --force hace un stop antes del rm.

## Subir una imagen a dockerhub

Lo primero que hay que hacer es autenticarse en dockerhub mediante:

    docker login

Después de la autenticación ya se puede subir una imagen que tengamos registradas en el registry local:

    docker image push $DOCKERID/linux_tweet_app:1.0

**NOTA:** Dockerhub es uno de los posibles registries que hay para docker.

## Enlaces

http://training.play-with-docker.com/
