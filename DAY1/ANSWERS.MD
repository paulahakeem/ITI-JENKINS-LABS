### 1- install jenkins with docker image
- ![1](https://user-images.githubusercontent.com/116673091/216728783-02df6c5d-e41a-45b6-931b-271f3776a4f4.png)
---
### 2- install role based authorization plugin
- ![2](https://user-images.githubusercontent.com/116673091/216728948-4bde8253-a4f9-4704-91c2-af65650a2be8.png)
---
### 3- create new user
- ![3](https://user-images.githubusercontent.com/116673091/216729314-44369664-7354-415d-bf15-33cbb54d5b6b.png)
---
### 4- create read role and assign it to the new user
- ![read](https://user-images.githubusercontent.com/116673091/216733590-2fdad656-85bf-4b32-abcf-acbf3f6f0b92.png)
- ![read3](https://user-images.githubusercontent.com/116673091/216733729-0b3c6139-957f-47a1-b1fd-ba4427db9d58.png)
---
### 5- create free style pipeline and link it to private git repo(inside it create directory and create file with "hello world")
- shell commands![5 1](https://user-images.githubusercontent.com/116673091/216733842-fbd8e906-aee2-473b-a774-76cb92308c28.png)
- after exec in container ![5 2](https://user-images.githubusercontent.com/116673091/216733866-ba39589b-a707-400b-9577-1677188ee89f.png)
- output from jenkins ![5 3](https://user-images.githubusercontent.com/116673091/216733896-a05af7e4-c77c-43e0-9ac4-c5f299070225.png)
---
### 6-  create declarative in jenkins GUI pipeline for your own repo to do "ls"
- script
``` bash 
pipeline {
    agent any
    stages {
        stage('List Files') {
            steps {
                sh 'ls'
                echo 'job done'
            }
        }
    }
}
```
- screen shot from output ![eee](https://user-images.githubusercontent.com/116673091/216734836-1b46b92c-d66f-401c-b330-46aa0e58be47.png)
---
### 7- create scripted in jenkins GUI pipeline for your own repo to do "ls"
-script 
```bash
node {
    stage('List Files') {
        sh 'ls'
        echo 'job done scripted way'
    }
}
```
- screenshot from output ![777](https://user-images.githubusercontent.com/116673091/216736000-879876e4-fce3-4214-bf6e-d763a66cc0ce.png)
---
### 8- create the same with jenkinsfile in your branches as multibranch pipeline
![2](https://user-images.githubusercontent.com/116673091/216739566-bf822539-268e-434e-8fe2-2bbc9d2f62cc.png)
---
### 9- configure jenkins image to run docker commands on your host docker deamon
i used this to installl docker deamon in jenkins image
``` Dockerfile
# Use the official Jenkins image as the base image 
FROM jenkins/jenkins:lts 
 
# Install the Docker CE in the Jenkins container 
USER root 
RUN apt-get update && apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common 
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - 
RUN add-apt-repository \ 
    "deb [arch=amd64] https://download.docker.com/linux/debian \ 
    $(lsb_release -cs) \ 
    stable" 
RUN apt-get update && apt-get install -y docker-ce 
 
RUN usermod -aG docker jenkins
```
---
### 10- create CI/CD for this repo https://github.com/mahmoud254/jenkins_nodejs_example.git
- screenshot from Console Output ![jenklab](https://user-images.githubusercontent.com/116673091/217446793-408ff62c-6e34-49a8-b1ce-88f42dd88534.png)
- screenshot from jenkins ui ![de](https://user-images.githubusercontent.com/116673091/217447136-226898e0-a802-4ef5-82bf-091f9f6d7650.png)
- screenshot from browser ![dd](https://user-images.githubusercontent.com/116673091/217446950-e4f31946-bf5e-424b-a9f9-893837f10c62.png)
---
### 11- create docker file to build image for jenkins slave
Dockerfile 
```Dockerfile
FROM ubuntu:20.04
RUN apt-get update -qq
RUN apt install openjdk-11-jdk -y
## install ssh
RUN apt-get install openssh-server -y
RUN service ssh start
#install docker client
RUN apt-get update -y
RUN apt-get install -qq apt-transport-https ca-certificates  curl  gnupg lsb-release
RUN mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get update && apt-get install -y docker-ce 
###########################################################################
RUN mkdir /jenkins
RUN chmod 777 jenkins
WORKDIR /jenkins
CMD [ "/bin/bash" ]
```
---
### 12- create container from this image 
```bash
docker run -it -d -v /var/run/docker.sock:/var/run/docker.sock [image_name_after_build]
```
---
## and configure ssh
```bash
docker exec -it [containerID] bash
ssh-keygen
cd ./.ssh
cp id_rsa.pub authorized-keys 
```
---
### 13- from jenkins master create new node with the slave container
- screenshot from nodes page![22](https://user-images.githubusercontent.com/116673091/217757750-b2500a64-a5db-4c32-8c57-224071425f39.png)
- screenshot from logs page ![ccc](https://user-images.githubusercontent.com/116673091/217757890-b1e1eddd-fa0a-4795-8b56-c7b1737e08c8.png)
---
### 14 - integrate slack with jenkins
- screenshot from slack![sccclack](https://user-images.githubusercontent.com/116673091/217783933-c4a6df2e-3065-4aff-b56f-6650aec46999.png)
---
### 15 - send slack message when stage in your pipeline is successful
- screenshot from slack ![Screenshot from 2023-02-09 12-15-49](https://user-images.githubusercontent.com/116673091/217784173-4bd855c5-8fe2-4c9f-b6e4-7721ce60b9c6.png)
---

