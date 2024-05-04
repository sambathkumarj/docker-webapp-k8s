# docker-webapp-k8s

# Prerequesties

Virtual Machine (RAM -4GB)
Docker Hub account
Kubernetes cluster - Microk8s


# DOCKER

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications.

Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime.

# Install Docker Engine on Ubuntu

To get started with Docker Engine on Ubuntu, make sure you meet the prerequisites, and then follow the installation steps.

# Prerequisites

Note
If you use ufw or firewalld to manage firewall settings, be aware that when you expose container ports using Docker, these ports bypass your firewall rules. For more information, refer to Docker and ufw.
OS requirements
To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:
•	Ubuntu Noble 24.04 (LTS)
•	Ubuntu Mantic 23.10 (EOL: July 12, 2024)
•	Ubuntu Jammy 22.04 (LTS)
•	Ubuntu Focal 20.04 (LTS)


Docker Engine for Ubuntu is compatible with x86_64 (or amd64), armhf, arm64, s390x, and ppc64le (ppc64el) architectures.
Uninstall old versions
Before you can install Docker Engine, you need to uninstall any conflicting packages.
Distro maintainers provide unofficial distributions of Docker packages in APT. You must uninstall these packages before you can install the official version of Docker Engine.
The unofficial packages to uninstall are:
•	docker.io
•	docker-compose
•	docker-compose-v2
•	docker-doc
•	podman-docker
Moreover, Docker Engine depends on containerd and runc. Docker Engine bundles these dependencies as one bundle: containerd.io. If you have installed the containerd or runc previously, uninstall them to avoid conflicts with the versions bundled with Docker Engine.


Run the following command to uninstall all conflicting packages:
$ for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
apt-get might report that you have none of these packages installed.
Images, containers, volumes, and networks stored in /var/lib/docker/ aren't automatically removed when you uninstall Docker. If you want to start with a clean installation, and prefer to clean up any existing data, read the uninstall Docker Engine section.


Installation methods
You can install Docker Engine in different ways, depending on your needs:
•	Docker Engine comes bundled with Docker Desktop for Linux. This is the easiest and quickest way to get started.
•	Set up and install Docker Engine from Docker's apt repository.
•	Install it manually and manage upgrades manually.
•	Use a convenience script. Only recommended for testing and development environments.


# Install using the apt repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.
1.	Set up Docker's apt repository.
   
# Add Docker's official GPG key:

```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

# Add the repository to Apt sources:

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Note
If you use an Ubuntu derivative distro, such as Linux Mint, you may need to use UBUNTU_CODENAME instead of VERSION_CODENAME.

 2.	Install the Docker packages.

# To install the latest version, run:
```
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3.	Verify that the Docker Engine installation is successful by running the hello-world image.
```
$ sudo docker run hello-world
```

This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
You have now successfully installed and started Docker Engine.

# Root access for docker.

1) Create the docker group
``` 
 $ sudo groupadd docker
```

2) Add your user to the docker group
 ```
 $ sudo usermod -aG docker $USER
```

# Command for pull images from docker hub reposiforry :

```
$ docker image pull nginx:latest [docker image name]
```

Command for create and run docker image as a container :
```
 $ docker run -d[background run] -it[intractive terminal] -p[port] 80[user assign port]:80 [container name] [image name]
```
 Command for show list of images : 
 ```
 $ docker images
```
Command for show running containers : 
```
$ docker ps 
```
Command for find image, container, size and active status : 
```
$ docker system df
```
 Command for find errors :
 ```
 $ docker system events
```
Command for find docker information: 
```
$ docker system info
```
Command for docker search in reposiforry :
```
$ docker search [image name]
```
Command for remove docker images :
```
$ docker rmi docker [image name or id]
$ docker rmi -f [for force delete]
```
Command for remove container : before remove container need for sforp container
```
$ docker rm [container name or id]
```
Command for sforp docker container :
```
$ docker sforp [container or id]
```
Command for start docker container :
```
$ docker start [container or id]
```
Command for find docker logs :
```
$ docker logs [container name or id]
```
Command for show image config :
```
$ docker inspect [image id or name]
```
Command for backup image :'
```
$ docker save [image id or name]
```
 Command for extract backuped image:
``` 
 $ docker load -i file name.tar
```
Command for save changes in container :
```
$ docker commit
```
Command for push image in reposiforry :
```
$ docker push [image name]:tag
```
Command for rename image :
```
$ docker tag [image name:tag] [rename:tag]
```

# Steps to create your own docker image:

Create a directory to stores your Dockerfile and your web applications files
```
mkdir webpage
```
Create a dockerfile :
```
sudo nano dockerfile

# Copy and Paste

FROM nginx:latest
COPY index.html /usr/share/nginx/html/
COPY netflixstyles.css  /usr/share/nginx/html/
EXPOSE 80
```

Then push your Web applications file like .html, .css, .js to the webpage directory

Docker Hub Login:(Create account)
```
docker login
#username and passwd
```

Create the repository to push the local image to the docker hub,
Then,
```
docker build -t sambathkumarj/netflix<Image name> .
```

# After creating image, check using this command

```
docker images
docker push  sambathkumarj/netflix
docker run -d --name <Container name> web  sambathkumarj/netflix
docker start <container-name>
```

# Deploy the image with Kubernetes cluster - Microk8s

```
microk8s kubectl create deployment netflixweb –image=sambathkumarj/netflixweb

deployment.apps/netflixweb created
```
# Create a service that exposes to the deployment created as NodePort
```
microk8s kubectl expose deployment netflixweb --type=NodePort --port=80 –name=netflix-service
service/netflix-service exposed
```

# Browse with localhost IP with the port generated with NodePort

http://<IP-Addresss>:32044 <Nodeport{port}>

