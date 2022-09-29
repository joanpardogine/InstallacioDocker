# Instal·lació d'Ubuntu 22.04

## Descarregar la ISO de ubuntu 22.04

https://ubuntu.com/download/server

https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-live-server-amd64.iso?_ga=2.133393614.1105979647.1664448107-1376380089.1664211634

ubuntu-22.04.1-live-server-amd64.iso


# Instal·lació del Docker Engine a Ubuntu

https://docs.docker.com/engine/install/ubuntu/

## Requisits previs 

### Requisits del sistema operatiu 
Per instal·lar Docker Engine, necessiteu la versió de 64 bits d'una d'aquestes versions d'Ubuntu:

 * Ubuntu Jammy 22.04 (LTS)
 * Ubuntu Impish 21.10
 * Ubuntu Focal 20.04 (LTS)
 * Ubuntu Bionic 18.04 (LTS)

Docker Engine és compatible amb les arquitectures ```x86_64```(o ```amd64```), ```armhf```, ```arm64i``` i ```s390x```.

### Desinstal·leu les versions antigues 
Les versions anteriors de Docker es deien docker, docker.io, o docker-engine. Si estan instal·lats, desinstal·leu-los:

 ```sudo apt-get remove docker docker-engine docker.io containerd runc```

Està bé si ```apt-get``` informa que cap d'aquests paquets està instal·lat.

Es conserva el contingut de ```/var/lib/docker/```, incloses imatges, contenidors, volums i xarxes.

## Instal·lació a travès del repositori 

Abans d'instal·lar Docker Engine per primera vegada en una màquina amfitrió nova, heu de configurar el repositori de Docker. Després, podeu instal·lar i actualitzar Docker des del repositori.

### Configura el repositori

**1.** Actualitzeu els índex dels paquets d'```apt``` i instal·leu els següents paquets per permetre utilitzar un repositori mitjançant HTTPS:

```
$ sudo apt-get update
$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

**2.**  Afegeix la clau GPG oficial de Docker:
```
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

**3.** Utilitzeu l'ordre següent per configurar el repositori:
```
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Instal·leu Docker Engine

**1.** Actualitzeu els índex dels paquets d'```apt``` i instal·leu la *darrera versió* (***latests version***) de ```Docker Engine```, ```containerd``` i ```Docker Compose```, o aneu al pas següent per instal·lar una versió específica:

```
$ sudo apt-get update
$ sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

> ### Rebeu un error de GPG quan s'executeu apt-get update?
> 
> És possible que la vostra ```umask``` predeterminada no s'hagi configurat correctament, cosa que fa que no es detecti el fitxer de clau pública del repositori. Executeu l'ordre següent i torneu a provar d'actualitzar el vostre repositori:
> ```sudo chmod a+r /etc/apt/keyrings/docker.gpg```

**2.** Per instal·lar una versió específica de Docker Engine, enumereu les versions disponibles al repositori i, a continuació, seleccioneu i instal·leu:

  **2. a.** Enumereu les versions disponibles al vostre repositori:

```
$ apt-cache madison docker-ce

docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
docker-ce | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
docker-ce | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages

```
  **2. b.** Instal·leu una versió específica mitjançant la cadena de versió de la segona columna, per exemple, ```5:20.10.16~3-0~ubuntu-jammy```.

```
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin
```


**3.** Comproveu que ***```Docker Engine```*** estigui instal·lat correctament.

```
$ sudo service docker start
```

### Sortida de l'estat del servei

```
$ sudo service docker status
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-09-29 14:03:30 UTC; 11min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 1910 (dockerd)
      Tasks: 8
     Memory: 21.9M
        CPU: 851ms
     CGroup: /system.slice/docker.service
             └─1910 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Sep 29 14:03:28 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:28.137014936Z" level=info msg="scheme \"uni>
Sep 29 14:03:28 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:28.137120818Z" level=info msg="ccResolverWr>
Sep 29 14:03:28 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:28.137222500Z" level=info msg="ClientConn s>
Sep 29 14:03:29 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:29.027944387Z" level=info msg="Loading cont>
Sep 29 14:03:29 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:29.901298776Z" level=info msg="Default brid>
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.212340325Z" level=info msg="Loading cont>
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.255102215Z" level=info msg="Docker daemo>
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.255309338Z" level=info msg="Daemon has c>
Sep 29 14:03:30 ubuntu-docker systemd[1]: Started Docker Application Container Engine.
lines 1-21...skipping...
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-09-29 14:03:30 UTC; 11min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 1910 (dockerd)
      Tasks: 8
     Memory: 21.9M
        CPU: 851ms
     CGroup: /system.slice/docker.service
             └─1910 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Sep 29 14:03:28 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:28.137014936Z" level=info msg="scheme \"uni>
Sep 29 14:03:28 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:28.137120818Z" level=info msg="ccResolverWr>
Sep 29 14:03:28 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:28.137222500Z" level=info msg="ClientConn s>
Sep 29 14:03:29 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:29.027944387Z" level=info msg="Loading cont>
Sep 29 14:03:29 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:29.901298776Z" level=info msg="Default brid>
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.212340325Z" level=info msg="Loading cont>
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.255102215Z" level=info msg="Docker daemo>
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.255309338Z" level=info msg="Daemon has c>
Sep 29 14:03:30 ubuntu-docker systemd[1]: Started Docker Application Container Engine.
Sep 29 14:03:30 ubuntu-docker dockerd[1910]: time="2022-09-29T14:03:30.340413393Z" level=info msg="API listen o>
```

**4.** Comproveu que ***```Docker Engine```*** estigui instal·lat correctament executant la imatge ```hello-worldimatge```.

```
$ sudo docker run hello-world
```

### Sortida de l'execució de la imatge ```hello-worldimatge```.

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:62af9efd515a25f84961b70f973a798d2eca956b1b2b026d0a4a63a3b0b6a3f2
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```


Aquesta ordre descarrega una imatge de prova i l'executa en un contenidor. Quan el contenidor s'executa, imprimeix un missatge (```Hello from Docker!```) i surt.

**```Docker Engine```** està instal·lat i en funcionament. El grup ```docker``` s'ha creat però no s'hi afegeix cap usuari. Heu de fer servir **```sudo```** per executar les ordres de **Docker**. Continueu amb la postinstal·lació de Linux per permetre als usuaris sense privilegis executar ordres de **Docker** i altres passos de configuració opcionals.


### Actualitza el motor **Docker**

Per actualitzar **Docker Engine**, primer executeu **```sudo apt-get update```**, seguiu les [instruccions d'instal·lació](./README.md#installació-a-travès-del-repositori) i escolliu la versió nova que voleu instal·lar.

### Desinstal·leu **Docker Engine**

Desinstal·leu els paquets ```Docker Engine```, ```CLI```, ```Containerd``` i ```Docker Compose```:

```
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Les imatges, els contenidors, els volums o els fitxers de configuració personalitzats del vostre amfitrió **NO** s'eliminen automàticament. Per suprimir totes les imatges, contenidors i volums:

```
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

Heu de suprimir els fitxers de configuració editats manualment.


