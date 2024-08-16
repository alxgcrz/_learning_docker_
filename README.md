# Docker

> :warning: **DOCUMENTO EN DESARROLLO** :warning:

## Overview

Docker permite empaquetar las aplicaciones en **contenedores** que incluyen todo lo necesario para que se puedan ejecutar en un **entorno aislado**. Cada contenedor almacena el código fuente de la aplicación, los archivos de configuración y todas las dependencias de software que necesita. Esta estrategia permite que las aplicaciones se puedan ejecutar de la misma manera sobre cualquier infraestructura que tenga soporte para Docker, tanto de forma local como en la nube.

Con la tecnología de contenedores para aplicaciones, ya no es necesario preocuparse por el software que está instalado en la máquina donde se ejecuta el contenedor, porque **todo lo que la aplicación necesita** está incluido dentro del propio contenedor. Esta forma de trabajar resuelve el problema de _'funciona en mi máquina'_, donde una aplicación puede funcionar correctamente en el entorno de desarrollo, pero tiene errores en el entorno de producción porque los dos entornos no son idénticos y contienen versiones de software diferentes.

Cada vez hay más equipos de desarrollo y operaciones que están utilizando la tecnología de contenedores Docker en sus flujos de trabajo. Esto ha permitido acelerar el proceso de desarrollo de las aplicaciones, ha facilitado la forma de distribuirlas y ha acelerado la automatización del despliegue en producción.

### Máquina virtual vs contenedor

Una **máquina virtual (VM)** es un entorno que emula la misma funcionalidad de una máquina física. Una máquina virtual utiliza los **recursos** que se le han asignado, como por ejemplo su propia CPU, memoria, interfaz de red, almacenamiento y su propio sistema operativo.

Las máquinas virtuales (VM) se crean y se ejecutan sobre un software llamado **hipervisor** o **_virtual machine monitor_ (VMM)**. El hipervisor se ejecuta en la máquina física y actúa como una **capa intermedia** entre el hardware de la máquina anfitriona o _'host'_ y la máquina virtual. El hipervisor se encarga de **gestionar y distribuir** los recursos de la máquina física entre las máquinas virtuales.

Es posible crear varias máquinas virtuales sobre una misma máquina física. Cada una de las máquinas virtuales estará aislada del resto, tendrá sus propios recursos y contará con su **propio sistema operativo**, que no necesariamente será el mismo que el de la máquina anfitriona.

Un **contenedor** se puede definir como una **unidad estándar de software** que permite empaquetar el código fuente de una aplicación y todas sus dependencias, para que se pueda **distribuir y ejecutar** de forma rápida y fiable en diferentes entornos.

También se puede definir como un proceso que ha sido aislado de todos los demás procesos de la máquina anfitriona en la que está ejecutando. Aunque es posible tener más de un proceso en un contenedor, las buenas prácticas recomiendan ejecutar solo un proceso por contenedor.

Los contenedores deben cumplir con los **estándares abiertos** de la industria de los contenedores software desarrollados por la **OCI (_Open Container Initiative_)**.

La principal diferencia entre una máquina virtual y un contenedor es que la **máquina virtual necesita un sistema operativo** completo para poder funcionar mientras que un **contenedor no lo necesita** ya que comparte el kernel del sistema operativo de la máquina en la que se está ejecutando.

Por lo tanto, **los contenedores requieren menos recursos** que las máquinas virtuales. Con el mismo hardware es posible tener un mayor número de contenedores que de máquinas virtuales.

Además, los contenedores son más livianos y arrancan más rápido que las máquinas virtuales.

Por último, **un contenedor se puede ejecutar dentro de una máquina virtual** pero no al revés.

## La arquitectura Docker

La arquitectura de Docker fue diseñada inicialmente de forma monolítica, pero más tarde fue rediseñada hacia una **arquitectura modular**, compuesta por diferentes componentes que pueden ser reemplazados e incluso utilizados en otros proyectos.

Cada uno de los componentes de Docker se desarrolla por separado y muchos de ellos forman parte del [**"Proyecto Moby"**](https://mobyproject.org/), un proyecto _open source_ creado por la compañía Docker Inc. en 2017, en el cual se desarrollan componentes y herramientas que pueden ser utilizadas para crear productos basados en la tecnología de contenedores.

Los principales componentes de Docker que debemos conocer son:

- Cliente Docker
  - Docker CLI
  - Docker Compose
- Docker Engine
  - Docker Engine API
  - Docker Daemon
- Container Runtime
  - Containerd
  - Runc
- Docker Registry

<!-- markdownlint-disable MD033 -->
<p align="center">
    <img src="assets/docker-architecture.png" alt="docker architecture">
</p>
<!-- markdownlint-enable MD033 -->

### Cliente Docker

Docker utiliza una arquitectura **cliente-servidor**, donde una aplicación cliente interactúa con un servicio llamado **_Docker Daemon_**. Un mismo cliente puede comunicarse con más de un servicio **_Docker Daemon_**.

La comunicación entre cliente y servidor se realiza a través de una API HTTP conocida como **_Docker Engine API_**.

Las aplicaciones oficiales que se pueden utilizar como cliente son **Docker CLI (_'Command Line Interface'_)** y **_Docker Compose_** aunque cualquier aplicación cliente que haga uso de la API de **_Docker Engine_** puede ser un cliente válido.

El cliente y el servidor se pueden ejecutar en la **misma máquina** o en **máquinas separadas**. Cuando están en la misma máquina, la comunicación entre ambos se realiza a través de un **socket IPC** o un **socket TCP**. En cambio, cuando se encuentran en máquinas separadas, la comunicación se realiza mediante un **socket TCP**.

#### Docker CLI

**_Docker CLI_** es el cliente oficial de Docker. Es una interfaz de línea de comandos que permite a los usuarios interactuar con el servicio **_Docker Daemon_**.

```sh
# Muestra la ayuda de Docker
$ docker help
```

El uso más habitual de **_Docker CLI_** es cuando se desea interactuar con un **único contenedor**.

#### Docker Compose

**_Docker Compose_** es una aplicación utilizada desde la línea de comandos que permite a los usuarios interactuar con el servicio **_Docker daemon_**.

```sh
# Muestra la ayuda de Docker Compose
$ docker compose help
```

Esta aplicación permite definir y ejecutar aplicaciones con **múltiples contenedores**. Utiliza un archivo de configuración con formato YAML para definir los servicios, las redes y los volúmenes que componen la aplicación que se desea ejecutar.

Una de las ventajas que ofrece **_Docker Compose_** es que basta con ejecutar un solo comando para crear y ejecutar todos los servicios definidos en el archivo de configuración en formato YAML.

En la actualidad existen dos versiones de **_Docker Compose_**:

- **v1**: debe instalarse como una herramienta adicional y se ejecuta con `docker-compose`

- **v2**: integra el comando `compose` dentro del cliente oficial de **_Docker CLI_**. Por lo tanto, la nueva versión se ejecuta con `docker compose`

### Docker Engine

**_Docker Engine_** es el componente principal de Docker, responsable de crear, ejecutar y gestionar contenedores.

Tiene un diseño **modular** y está compuesto por varios componentes que cumplen con los estándares abiertos de la **OCI (_Open Container Initiative_)**:

- _Docker Engine API_

- _Docker Daemon_

- _Container runtime_ (_containerd_)

- Gestión de redes (_libnetwork_)

- Creación de imágenes (_buildkit_)

- Interacción con los registros de contenedores (_distribution_)

- Soporte nativo para la orquestación de contenedores con Docker Swarm (_swarmkit_)

- Gestión de plugins

> :warning: Estos son proyectos que cumplen los estándares abiertos y están alojados en [**"Moby Project"**](https://mobyproject.org/projects/).

**_Docker Engine_** se ejecuta de forma nativa en los sistemas Linux y Windows Server. En otros sistemas operativos de Windows y en macOS, se ejecuta sobre una máquina virtual Linux.

#### Docker Engine API

(TODO)

#### Docker daemon

(TODO)

#### Container runtime

##### _containerd_

(TODO)

##### _runc_

(TODO)

### Docker Registry

(TODO)

## Objetos de Docker

Los principales objetos de Docker son:

- Imágenes
- Contenedores
- Volúmenes
- Redes

### Imágenes

Las imágenes son **plantillas inmutables** que contienen el sistema de archivos y los parámetros necesarios para configurar un contenedor. Representan el estado inicial del sistema de archivos raíz del contenedor y pueden incluir aplicaciones preinstaladas, configuraciones y dependencias.

Las imágenes se pueden crear manualmente utilizando un archivo llamado `Dockerfile`, que contiene una serie de instrucciones para ensamblar la imagen paso a paso. También es posible obtener imágenes predefinidas desde registros públicos como **_Docker Hub_**.

Docker utiliza un **sistema de capas** para construir imágenes, lo que permite reutilizar partes comunes entre diferentes imágenes y optimizar el almacenamiento y la transferencia.

A partir de una misma imagen, se pueden **crear múltiples contenedores**, lo que facilita la escalabilidad y consistencia en la implementación de aplicaciones.

### Contenedores

Un contenedor es una **instancia ejecutable de una imagen**. Representa una unidad de software ligera y autónoma que incluye todo lo necesario para ejecutar una aplicación.

Puede crear, iniciar, detener, mover o eliminar un contenedor utilizando la API o la CLI de Docker. Además, es posible conectar un contenedor a una o más redes, adjuntar almacenamiento persistente o incluso crear una nueva imagen basada en el estado actual del contenedor mediante la ejecución de comandos como `docker commit`.

De forma predeterminada, un contenedor está relativamente **aislado** de otros contenedores y de la máquina _host_. Este aislamiento incluye el sistema de archivos, la red y otros recursos del sistema. Sin embargo, Docker proporciona opciones para controlar el nivel de aislamiento según las necesidades específicas.

Es importante notar que, a menos que se utilicen volúmenes o montajes de enlaces, los cambios realizados dentro de un contenedor no se preservan una vez que el contenedor se elimina.

### Volúmenes

Los volúmenes son el mecanismo que ofrece Docker para gestionar la **persistencia de datos** en los contenedores. Permiten almacenar datos fuera del sistema de archivos del contenedor, asegurando que la información persista incluso si el contenedor se elimina.

Los volúmenes se pueden crear y gestionar utilizando comandos de Docker, como `docker volume create`. Una vez creados, pueden ser montados en uno o varios contenedores.

El ciclo de vida de los volúmenes es independiente del ciclo de vida de los contenedores. Esto significa que los volúmenes permanecen **intactos y disponibles** para otros contenedores incluso después de que los contenedores que los utilizaban hayan sido eliminados.

Además de los volúmenes, Docker también soporta montajes de enlaces (_'bind mounts'_), que permiten montar directorios o archivos específicos del sistema de archivos de la máquina host dentro de un contenedor.

### Redes

Docker proporciona capacidades avanzadas para la creación y gestión de redes, facilitando la **comunicación entre contenedores y con sistemas externos**.

Docker soporta varios tipos de redes, incluyendo:

- **_Bridge_** ➜ Es la red por defecto en Docker. Permite la comunicación entre contenedores en la misma máquina host.

- **_Host_** ➜ Utiliza la pila de red de la máquina host, permitiendo que los contenedores compartan la misma dirección IP y puertos que el host.

- **_Overlay_** ➜ Permite la comunicación entre contenedores que residen en diferentes máquinas host, comúnmente utilizada en configuraciones de clústeres y orquestación.

- **_Macvlan_** ➜ Asigna una dirección MAC a cada contenedor, permitiendo que aparezcan como dispositivos físicos en la red.

A través de comandos como `docker network create`, `docker network connect` y `docker network inspect`, los usuarios pueden crear redes personalizadas, conectar contenedores a ellas y obtener información detallada sobre su configuración.

## Appendix: Common Docker Commands

### Images

```bash
# Comandos de 'image'
docker image --help

# Listar todas las imágenes instaladas localmente
# Aliases 'docker image list', 'docker images'
docker image ls [OPTIONS] [REPOSITORY[:TAG]]
 
# Mostrar información detallada sobre una imagen específica
docker image inspect [OPTIONS] IMAGE [IMAGE...]

# Ver historial de instrucciones de una imagen
# Aliases 'docker history'
docker image history [OPTIONS] IMAGE

# Etiquetar una imagen existente
# Aliases 'docker tag'
docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

# Eliminar una imagen específica
# Aliases 'docker image remove', 'docker rmi'
docker image rm [OPTIONS] IMAGE [IMAGE...]

# Eliminar todas las imágenes
docker image rm $(docker image ls --all --quiet)  # -aq

# Eliminar imágenes sin etiqueta (o 'dangling') no asociadas a contenedores activos
docker image prune [OPTIONS]

# Eliminar imágenes sin etiquetar e imágenes etiquetadas no asociadas a contenedores activos
docker image prune --all
```

#### Docker Hub

```bash
# Autenticarse en el registro predeterminado de Docker (Docker Hub)
docker login [OPTIONS]

# Autenticarse en un registro local como puede ser un servidor local
docker login [OPTIONS] [SERVER]

# Cerrar sesión
docker logout

# Cerrar sesión de un registro local
docker logout [SERVER]

# Buscar una imagen por algún término en Docker Hub
docker search [OPTIONS] TERM

# Descarga una imagen de Docker Hub
# Aliases 'docker pull'
docker image pull [OPTIONS] NAME[:TAG|@DIGEST]

# Subir una imagen a Docker Hub
# Aliases 'docker push'
docker image push [OPTIONS] NAME[:TAG]
```

### Containers

```bash
# Comandos de 'container'
docker container --help

# Listar contenedores en ejecución
# Aliases 'docker container list', 'docker container ps', 'docker ps'
docker container ls [OPTIONS]

# Listar todos los contenedores
docker container ls --all

# Listar todos los contenedores que han salido con un estado específico
docker container ls -a --filter "status=exited"

# Obtener el ID del contenedor para una imagen específica usando una expresión regular
docker container ls | grep wildfly | awk '{print $1}'

# Ver detalles del contenedor
docker container inspect [OPTIONS] CONTAINER [CONTAINER...]

# Encontrar la IP de un contenedor
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' CONTAINER

# Mostrar los logs del contenedor
# Aliases 'docker logs'
docker container logs [OPTIONS] CONTAINER

# Mostrar últimos logs
# Aliases 'docker logs'
docker container logs --tail 100 CONTAINER

# Ver estadísticas en tiempo real
# Aliases 'docker stats'
docker container stats [OPTIONS] [CONTAINER...]

# Ejecutar un comando en el contenedor
# Aliases 'docker exec'
docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]

# Renombrar un contenedor
# Aliases 'docker rename'
docker container rename CONTAINER NEW_NAME

# Conectar a un contenedor
# Aliases 'docker attach'
docker container attach [OPTIONS] CONTAINER

# Arrancar un contenedor
# Aliases 'docker start'
docker container start [OPTIONS] CONTAINER [CONTAINER...]

# Detener un contenedor
# Aliases 'docker stop'
docker container stop [OPTIONS] CONTAINER [CONTAINER...]

# Detener todos los contenedores en ejecución
docker container stop $(docker container ls --quiet)

# Eliminar un contenedor
# Aliases 'docker container remove', 'docker rm'
docker container rm [OPTIONS] CONTAINER [CONTAINER...]

# Eliminar contenedores que coincidan con una expresión regular
docker container ls -a | grep wildfly | awk '{print $1}' | xargs docker container rm -f

# Eliminar todos los contenedores que han salido
docker container rm -f $(docker container ls --all --filter "status=exited" --quiet)

# Eliminar todos los contenedores
docker container rm $(docker container ls --all --quiet)

# Crear y ejecutar un nuevo contendedor a partir de una imagen
# Aliases 'docker run'
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

# Crea y ejecuta un nuevo contenedor
docker container run --detach --name my-container --publish 80:80 my-image

# Abrir una shell interactiva en un contenedor
docker container exec -it ${CONTAINER_ID} bash
```

### Volume

```bash
# Comandos de 'volume'
docker volume --help

# Listar todos los volúmenes
# Aliases 'docker volume list'
docker volume ls

# Eliminar todos los volúmenes no utilizados
docker volume prune [OPTIONS]

# Eliminar volúmenes huérfanos (_'dangling'_)
docker volume rm $(docker volume ls --quiet --filter dangling=true) # -qf
```

### Others

```bash
# Mostrar el número de versión de Docker
docker --version

# Mostrar la versión del cliente y del servidor
docker version [OPTIONS]

# Muestra los registros del servicio de Docker en sistemas basados en 'systemd'
journalctl -u docker

## Comandos de 'system'
docker system --help

# Proporciona un resumen completo del estado del daemon de Docker
# Aliases 'docker info'
docker system info [OPTIONS]

# Muestra el uso de espacio en disco por imágenes, contenedores, y volúmenes
docker system df [OPTIONS]

# Limpieza general del sistema
docker system prune [OPTIONS]

# Iniciar el daemon manualmente
dockerd

# Verifica el estado del servicio Docker en sistemas que usan 'systemd'
sudo systemctl status docker

# Reinicia el servicio Docker  en sistemas que usan 'systemd'
sudo systemctl restart docker

# Permite especificar un archivo de configuración personalizado para el daemon de Docker
docker daemon --config-file /path/to/config.json

# Actualizar Docker a la última versión disponible
sudo apt-get update && sudo apt-get upgrade docker-ce
```

---

## Referencias

- <https://docs.docker.com/>
- <https://docs.docker.com/language/java/>
- <https://cheatsheets.zip/docker>
- <https://github.com/wsargent/docker-cheat-sheet/tree/master/es-es>
- <https://github.com/collabnix/dockerlabs/blob/master/docker/cheatsheet/README.md>

## Licencia

[![Licencia de Creative Commons](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)
Esta obra está bajo una [licencia de Creative Commons Reconocimiento-Compartir Igual 4.0 Internacional](http://creativecommons.org/licenses/by-sa/4.0/).
