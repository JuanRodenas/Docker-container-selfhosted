# Docker container selfhosted
Project to install Docker and Docker-Compose in Ubuntu, Raspberry or Nanopi

#

<p align="center">
    <a href="https://www.docker.com/">
        <img src="https://github.com/JuanRodenas/Docker-container-selfhosted/blob/main/Docker.png" alt="Docker">
    </a>
    <br>
    <strong>Make your projects come to life with Docker</strong>
</p>
<!-- markdownlint-enable MD033 -->

#

* Documentaci贸n oficial:
馃搧[Documentaci贸n docker](https://docs.docker.com/engine/)
馃搧[Documentaci贸n hub](https://docs.docker.com/docker-hub/)
馃搧[Documentaci贸n docker compose](https://docs.docker.com/compose/compose-file/compose-file-v3/#entrypoint)

## INSTALAR DOCKER EN UBUNTU

### INSTALAR DOCKER
Primero, actualice su lista de paquetes existente: 
~~~
sudo apt update
~~~

A continuaci贸n, instale algunos paquetes de requisitos previos que permitan a apt usar paquetes a trav茅s de HTTPS: 
~~~
sudo apt install apt-transport-https ca-certificates curl software-properties-common
~~~

Luego, a帽ada la clave de GPG para el repositorio oficial de Docker en su sistema: 
~~~
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
~~~

Agregue el repositorio de Docker a las fuentes de APT: 
~~~
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
~~~

A continuaci贸n, actualice el paquete de base de datos con los paquetes de Docker del repositorio reci茅n agregado: 
~~~
sudo apt update
~~~

Por 煤ltimo, instale Docker:
~~~
sudo apt install docker-ce
~~~

Compruebe que funcione: 
~~~
sudo systemctl status docker
~~~

Ejecutar el comando Docker sin sudo, si desea evitar escribir sudo al ejecutar el comando docker, agregue su nombre de usuario al grupo docker: 
~~~
sudo usermod -aG docker ${USER}
~~~

### Instalar Docker Compose
El siguiente comando descargar谩 la versi贸n 2.2.2 y guardar谩 el archivo ejecutable en /usr/local/bin/docker-compose, que har谩 que este software est茅 globalmente accesible como docker-compose, Si desea descargar la versi贸n m谩s reciente, ir al enlace: https://github.com/docker/compose/releases y modificar la versi贸n:

~~~
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
~~~
Le damos permisos de ejecuci贸n:
~~~
sudo chmod +x /usr/local/bin/docker-compose
~~~

Para verificar que la instalaci贸n se realiz贸 correctamente, puede ejecutar:

~~~
docker-compose --version
~~~

## INSTALAR DOCKER EN RASPBERRY O NANOPI

### Preparaci贸n de la Raspberry Pi
Instalar los kernel-headers para el sistema operativo. Esto es importante, ya que si no instala los encabezados del kernel, Docker no funcionar谩.
~~~
sudo apt install raspberrypi-kernel raspberrypi-kernel-headers
~~~

### INSTALAR DOCKER
Primero, actualice su lista de paquetes existente: 
~~~
sudo apt update
~~~

A continuaci贸n, instale algunos paquetes de requisitos previos que permitan a apt usar paquetes a trav茅s de HTTPS: 
~~~
sudo apt install apt-transport-https ca-certificates curl software-properties-common
sudo apt install -y libffi-dev libssl-dev python3 python3-pip
sudo apt install iptables-persistent
sudo apt install unattended-upgrades
~~~

Por 煤ltimo, instale Docker:
~~~
sudo curl -sSL https://get.docker.com | sh
~~~

Compruebe que funcione: 
~~~
sudo systemctl status docker
~~~

Ejecutar el comando Docker sin sudo, si desea evitar escribir sudo al ejecutar el comando docker, agregue su nombre de usuario al grupo docker: 
~~~
sudo usermod -aG docker ${USER}
~~~

### Instalar Docker Compose
El siguiente comando descargar谩 e instalar谩 docker-compose:
~~~
sudo apt install -y docker-compose
~~~

Para verificar que la instalaci贸n se realiz贸 correctamente, puede ejecutar:
~~~
docker-compose --version
~~~

## CONTENEDORES DOCKER
* [traefik](traefik/) - reverse proxy and SSL manager.
* [Adguard](Adguard/) -  Network-wide ads & trackers blocking DNS server.
* [Grafana](Grafana/) - The open-source platform for monitoring and observability.
* [Heimdall](Heimdall/) - Heimdall is an elegant solution to organise all your web applications.
* [Pihole](Pihole/) - The Pi-hole庐 is a DNS sinkhole that protects your devices from unwanted content without installing any client-side software.
* [Portainer](Portainer/) - Portainer is a lightweight service delivery platform for containerized applications that can be used to manage Docker, Swarm, Kubernetes and ACI environments.
* [syncthing](syncthing/) - Syncthing is a continuous file synchronization program.
* [wikijs](wikijs/) - Wiki.js is an open source project that has been made possible due to the generous contributions by community backers.
* [fail2ban](fail2ban/) - security tool (ban IP).
* [freshrss](freshrss/) - RSS feed aggregator.
* [gotify](gotify/) - notification service.
* [nextcloud](https://github.com/JuanRodenas/Nextcloud_server) - file-hosting software system.
* [transmission](transmission/) - fast, easy, and free BitTorrent client.
* [trilium](trilium/) - hierarchical note-taking application.
* [vaultwarden](vaultwarden/) - password manager.
* [watchtower](watchtower/) - automatic docker images update.
* [wireguard](https://github.com/JuanRodenas/Wireguard) - Wireguard is a selfhosted vpn.
* [wordpress](https://github.com/JuanRodenas/Wordpress) - WordPress is a blogging tool with a content management system (CMS).


# Information

The overall guide is centered around example. Each of the services is tied with either a docker-compose or a script, everything has been made so that each service is almost ready to use, only a few user-specific variable are required.

All services respect a certain format :

- **About** - basic overview of the service
- **Table of Contents**
- **Information** - detailed information about the service and the example
- **Usage** - required configuration and commands to use the service
- **Update** - how to update the container, most of the time it is using watchtower


## Docker and UFW

UFW is a popular iptables front end on Ubuntu that makes it easy to manage firewall rules. But when Docker is installed, Docker bypass the UFW rules and the published ports can be accessed from outside.

An [easy fix](https://github.com/chaifeng/ufw-docker) is available, allowing to easily manage your firewall. As most of the services are going through Traefik, only the port 443 is mandatory. If another port is required, it will be listed in the requirements.

## Docker tips

* Get shell access whilst the container is running
    ```
    docker exec -it container-name /bin/bash
    ```
* Monitor the logs of the container in realtime
    ```
    docker logs -f container-name
    ```

## Docker images

Most images are used with the tag `latest` as it simplify the testing. It is usually not recommended running an image with this tag as it is not very dynamic and precise.
Feel free to experiment with the provided docker-compose examples and then use a better versionning system. For more information about [latest](https://vsupalov.com/docker-latest-tag/).

## Updating docker images

This repository images are automatically updated with watchtower, however this can be a security risk. More details in the [watchtower guide](watchtower).

If you want to manually update an image, you can use docker-compose.

* Update all images for a specific docker-compose file
    ```
    sudo docker-compose pull
    ```
* Update a single image
    ```
    sudo docker-compose pull image-name
    ```
* Recreate all updated containers with docker-compose
    ```
    sudo docker-compose up -d
    ```
* Recreate a single container with docker-compose
    ```
    sudo docker-compose up -d container-name
    ```
* Remove all dangling and unused images
    ```
    sudo docker image prune  -a
    ```

## Docker tools

Some useful tools to manage your private docker infrastructure.

- [lazydocker](https://github.com/jesseduffield/lazydocker) - A simple terminal UI for both docker and docker-compose, written in Go with the gocui library. By @jesseduffield
- [dive](https://github.com/wagoodman/dive) - A tool for exploring each layer in a docker image. By @anchore.
- [grype](https://github.com/anchore/grype) - A vulnerability scanner for container images and filesystems. By @anchore.

## Docker resources 

A compilation of resources mainly focus on security.

- [CIS Docker 1.13.0 Benchmark](https://downloads.cisecurity.org/#/) - provides prescriptive guidance for establishing a secure configuration posture for Docker
- [Docker security](https://docs.docker.com/engine/security/) - official docker documentation about security
- [Docker security OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html) - OWASP security cheat sheet

### Credits
Buy me a coffee so I can continue creating content. Inv铆tame a un caf茅 para que pueda seguir creando contenidos.
#
<a href="https://www.paypal.com/donate/?hosted_button_id=HVJT2YDSHRZY2" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>
