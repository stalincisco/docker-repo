# Docker Notes
  1) Docker HUB on Google
  2) Download Postgres from docker hub page and install on docker, install different version
    - $ Docker ps
    - $ docker run postgres:9.6
    - $ docker run postgres:10.10
    
**Dockers installation steps**

  **Remove old dockers Files**

    $ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine


    Use DNF to add and enable the official Docker CE repository. Type the following command in your terminal window:
    $sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo

   **The system informs you that it has successfully retrieved the repository.**
   
    $sudo dnf repolist -v

    #To list all the available docker-ce packages, type:
   
    $dnf list docker-ce --showduplicates | sort -r

    Unfortunately, CentOS 8 does not support specific versions of the container.id package. This means that only some versions of docker-ce are available for installation.

## Install Docker CE on CentOS 8

 **Option 1: Skip Packages with Broken Dependencies**
 
   An efficient solution is to allow your CentOS 8 system to install the version that meets the criteria best, using the --nobest command:

   $sudo dnf install docker-ce --nobest

 **Option 2: Install containerd.io Package Manually**


 Another option for installing Docker on CenOS 8 is to install the containerd.io package manually, in advance. This workaround allows you to install the 
 latest docker-ce version.Use the following command:

   $sudo dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.10-3.2.el7.x86_64.rpm
 
 Confirm the installation with y. You have successfully installed the latest version of containerd.io.


 ## Now we can proceed to install the latest version of docker-ce with a simple command:

    $sudo dnf install docker-ce -y

 The output below confirms that docker-ce-3:19.03.5-3.el7.x86_64 has been successfully installed.

## Start and Test Docker
**Enable Docker**

Enable and start the Docker service with:

    $sudo systemctl enable --now docker

Next, use this short command to confirm that Docker is active and running:

    $systemctl status docker

 **Add User to Docker User Group**
 Add your user to the docker group with the following command:
    $sudo usermod -aG docker osboxes
    $id osboxes

  **Disable firewalld on CentOS 8**
    As mentioned previously, we need to disable firewalld for DNS resolution inside Docker containers to work.
    #One simple command is enough to disable firewalld in CentOS 8:

    $sudo systemctl disable firewalld

   At this point, it is recommended to reboot your system for the change to take effect.
    $sudo shutdown now  -r

    After your system is up in running use the below command
    
   **Start Docker.**

    $ sudo systemctl start docker

    Verify that Docker Engine is installed correctly by running the hello-world image.

    $ sudo docker run hello-world


**Optional : Uninstall Docker Engine**


    $ sudo yum remove docker-ce docker-ce-cli containerd.io

    Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

    $ sudo rm -rf /var/lib/docker
    $ sudo rm -rf /var/lib/containerd

   **Install Redis Application**
    
    $ docker pull redis

    $ docker images  ( will display images running on container)

    $ docker run redis

    $ docker ps  ( list running containers )
    Ctrl + C (Exit docker)

    $ docker run -d redis ( start a new container with a command )
    $ docker ps

    $ docker stop 838186(container ID) ( stop the container )
    $ docker ps
    
    $ docker start 838186(container ID) ( start the container )
    $ docker ps

    $ docker ps -a ( lists all the container which are running and stopped containers)

    $ docker run redis:4.0  ( pulls image and start container )
    $ docker ps

    $ docker run -p6000:6379  ( -p bind the port of your host to the container)
 
    $ docker run -p6000:6379 redis
    $ docker ps

    $ docker run -p6000:6379 -d redis
    $ docker ps
          
    $ docker run -p6001:6379 -d redis:4.0   ( run in detach mode, with port happed to 6001)
    $ docker ps

## How to Run a container with a name change
    $ docker ps  
    $ docker stop cfec85d7

    $ docker run -d -p6001:6379 --name redis-older redis:4.0
    $ docker ps
  
    (redis:4.0 image name will be changed to redis-older)

    $ docker run -d -p6000:6379 --name redis-latest redis
    $ docker ps

    (redis image name will be changed to redis-latest)

if older version has some issue I can do
    $ docker logs redis-older

To login to the terminal of a container and get log or troubleshoot, we can do
    $ docker exec -it Ce9032 /bin/bash
root@Ce9032 :/data# ls
root@Ce9032 :/data# pwd
/data
root@Ce9032 :/data# cd /
root@Ce9032 :/data# ls
Bin boot data

    root@Ce9032 :/# env
(displays environment variable)

    root@Ce9032 :/data# curl
    #(you will have limited number of commands which works under container)


##  Ngnix install

    $docker container create nginx
    $docker container ls -a

    $docker container inspect web01 | grep -e "HostPort" -e "IPAddress"
    $curl 172.17.0.4

    $docker container run -d --name web02 -dit -p 8080:80 nginx

    $docker container ls -sa

**Project**

1) Create an folder under /Home/app/
2) $git clone https://github.com/stalincisco/demo-repo/
3) extract zip folder under same directory (techworld-js-docker-demo-app-master)

## Download Mongo DB

    $ docker pull mongo
    $ docker pull mongo-express
    $ docket images ( to check existing images)

    Creating Docker Network
    $ docker network ls
    $ docker network create mongo-network
    $ docker network ls ( you will see mongo-network )


## Mongo DB install

**Commands**
 
   **Start mongodb**
    docker run -d \
    -p 27017:27017 \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=password \
    --net mongo-network \
    --name mongodb \
    mongo
    
## Mongo Express install

  **start mongo-express**
    docker run -d \
    -p 8081:8081 \
    -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
    -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
    -e ME_CONFIG_MONGODB_SERVER=mongodb \
    --net mongo-network \
    --name mongo-express \
    mongo-express


**to see MONGO Express, go to Firefox and type the below**
    Localhost:8081

 **create New database "my-db" and `users` in new collection database_ in mongo-express**
 **Start your nodejs application locally - go to `app` directory of project**

    $npm install 
    $node server.js
    
**to Access you nodejs application UI from Firefox browser**
    Localhost:3000

## Docker compose

    $ docker container kill web02 (kill all the application before launching dockers)
    $ docker-compose -f mongo.yaml up ( to bring the containers up)
    $ docker-compose -f mongo.yaml down ( to bring the containers down)


 **Remove the container**
    
    $ docker rm 3c5868bi (container name)
    $ docker rmi 2e08bi (image deletion)
    $ docker ps
    $ docker logs 34341FNADLFN (CONTAINER id)

**CONNECT TO A CONTAINER**
    
    $ docker exec -it 542524klksdfjad(container ID) /bin/sh
    /# ls
    /# env (mongo db username and password are set )
    /# ls /home/app/

    NodeJS is an open-source, cross-platform JavaScript runtime environment for developing network tools and web applications. Many of the NodeJS modules are written in  
    JavaScript which makes the development of applications easier.

    NodeJS is a combination of the Runtime environment and JavaScript modules. Node.js interprets JavaScript using Google’s V8 JavaScript engine, developed by Ryan Dahl in 2009.

    In this post, we will see how to install NodeJS on CentOS 8 / RHEL 8.
    NodeJS Versions

    There are two versions of NodeJS available for users. Check out the official page to find the latest version of Node.js.

        v12.x (Long Term Supported)
        v13.x (Current Latest Version)

   **Now install build tools To compile and install native addons from npm, you need to install development tools**
 
        yum groupinstall -y 'Development Tools'

   **Run the following command to add the package repository of NodeJS 12.x on your CentOS 8 machine:(Recommended)**
   
        $ curl -sL https://rpm.nodesource.com/setup_12.x | bash -
    	$ yum install nodejs

   **Run the following command to add the package repository of NodeJS 12.x on your CentOS 8 machine:(Recommended)**
   
        $ curl -sL https://rpm.nodesource.com/setup_13.x | bash -
    	$ sudo yum install nodejs

  ## Install NodeJS Using OS Repository

    NodeJS v10.x is available from the AppStream repository (rhel-8-for-x86_64-appstream-rpms) for RHEL 8 and AppStream for CentOS 8. So, you can simply install it
    using yum command.

    When NodeJS installed, NPM (Node Package Manager) will also be installed along with Node.js.
    	$ yum install -y @nodejs

   **Now run the following command to verify whether NodeJS is working**
   
        $ node -v

   **Now run the following command to see whether Node Package Manager (NPM) is working**
   
       $ npm -v

   **if you want to remove nodejs, execute the following command**
 
      $sudo yum remove -y nodejs npm

   **firewall**
   
         Allow the port 9000 in the firewall to access the web application from external machines.

    	$ firewall-cmd --permanent --add-port=9000/tcp

        $ firewall-cmd --reload

   **Now run the following command to start the app:**

        $ node --inspect server.js

   **The app should start.**
    	
	Now from your web browser, go to http://localhost:3000 and you should see the following output. The NodeJS app is working
    	correctly.
    	Now to stop the app, press <Ctrl> + c on the terminal.

## Docker build an image
   **how to create a Dockerfile**
  
         $ mkdir MyDockerImages

   **Move into that directory and create a new empty file (Dockerfile) in it by typing:**

    	$ cd MyDockerImages
    	$ touch Dockerfile

   **Open the file with a text editor of your choice. In this example, we opened the file using Nano:**
   
        $nano Dockerfile

    	FROM node:13-alpine
    	ENV MONGO_DB_USERNAME=admin \
    	    MONGO_DB_PWD=password
    	RUN mkdir -p /home/app
    	COPY ./app /home/app
   **set default dir so that next commands executes in /home/app dir**
    	WORKDIR /home/app
    **will execute npm install in /home/app because of WORKDIR**
    	RUN npm install
    **no need for /home/app/server.js because of WORKDIR**
    	CMD ["node", "server.js"]

**Save and exit the file.**

   You can check the content of the file by using the cat command:
   $ cat Dockerfile

## Note:

- FROM – Defines the base of the image you are creating. You can start from a parent image (as in the example above) or a base image. When using a parent image, you are using an existing image on which you base a new one. Using a base image means you are starting from scratch (which is exactly how you would define it: FROM scratch).
- RUN – Instructions to execute a command while building an image in a layer on top of it. In this example, the system searches for repository updates once it starts building the Docker image. You can have more than one RUN instruction in a Dockerfile.
- CMD – There can be only one CMD instruction inside a Dockerfile. Its purpose is to provide defaults for an executing container. With it, you set a default command. The system will execute it if you run a container without specifying a command.

**Build a Docker Image with Dockerfile**

   The basic syntax used to build an image using a Dockerfile is:
  
    $docker build [OPTIONS] PATH | URL | -
    
   To build a docker image, you would therefore use:
    
    $docker build [location of your dockerfile]
    
   If you are already in the directory where the Dockerfile is located, put a . instead of the location:
    
    $docker build .

   By adding the -t flag, you can tag the new image with a name which will help you when dealing with multiple images:

     $ docker build -t my-app:1.0 .                     (. Is for local install)

  **To see your image**

    $ docker Images

   **Create a New Container**

    $ docker run --name test my-app:1.0
	
##Install Visual Studio Code

**Switch to the root user.**
      
    $su -
**Download and import the Microsoft signing GPG key using the curl command.**

$rpm --import https://packages.microsoft.com/keys/microsoft.asc
   
 **Now, add the Visual Studio Code repository to your system.**

$cat << EOF > /etc/yum.repos.d/vscode.repo
[code]
name=Visual Studio Code
baseurl=https://packages.microsoft.com/yumrepos/vscode
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc
EOF

**Install Visual Studio Code**
  $yum check-update
  $yum install -y code
**Start Visual Studio Code**
  $code
**Update Visual Studio Code**
  $yum update code
  
## Install Cockpit on CentOS 8 
**Install the Cockpit package in case the package is not already installed.**
  
  $ dnf install -y cockpit

**You can install add-on packages to manage other tasks using Cockpit.**

  $ Enable the Cockpit service.
  $ systemctl enable --now cockpit.socket
  
**Firewall**

 $ firewall-cmd --permanent --add-service=cockpit
 $ firewall-cmd --reload

**Access Cockpit from Firefox**

https://ip.add.re.ss:9090

You can manage disks, partitions, and LVM by going to Storage (dnf install -y cockpit-storaged). Also, you can check disk read and write performance and storage logs.

Manage the Kernel virtual machines (dnf install -y cockpit-machines) by going to Virtual Machines.


**Adding mongodb docker with persistant volumes &  Authentication**

  $ docker run -d --name mongodb -p 27017:27017 -v mongo_db:/data/db mongo:latest 

  $ docker Volume ls 

**Default location for the Persistent Volumes under you host folders**

  $ cd /var/lib/docker/volumes/mongo_db/_data/
  $ ls 

**Now you should be able to see the data from the mongo db container**


**Connect to the Database**

  $ docker ps 
  $ docker exec -it containername /bin/bash

## Mongo shell commands

  $ mongo

(connect to the database) 

> mongo --help 

(displays detail of parameter used with mongo command)

> exit

(exit the DB)

>show dbs

>show databases

(displays details of the DB avaiable)

>use mongodb

**will create if new DB does not exit, will connect to db if it already exist.** 

>show collections 

**will show data collection on the DB**

> db.createCollection("newcollection")

**will create new collection data to the DB**

>show collections

**Output:** 

newcollection

>db.newcollection.find()

**to find collection on the db**

> db.newcollection.insert({firstname: "john", lastname: "alan"})

**to insert data in to the collection.**

>db.newcollection.find()

(again run the find commmand to see the output)

**install and add field in mongo db compass in windows, then check using find command the new fields added to the collection.**

>db.newcollection.find()

>exit

 $ docker ps

 $ docker rm -f containerID
**(remove the container and next remove the docker volumes)**

 $ docker volumes ls

 $ docker volumes rm mongo_db 434343(volume name)
**to remove the docker persistant volume.**
