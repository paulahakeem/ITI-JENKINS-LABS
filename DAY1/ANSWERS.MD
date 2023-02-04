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

