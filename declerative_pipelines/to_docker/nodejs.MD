```
pipeline {
    agent any
    tools {
  nodejs 'nodejs'
}

    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/arghya-roy/node-app.git'
            }
        }
        // stage('npm build') {           this is commanded because we use npm install in docker container by dockerfile
        //     steps {
        //         sh 'npm install'
        //     }
        // }
        stage('build docker image') {
            steps {
                sh 'docker build -t arghyaroy123/node1:v1.$BUILD_ID .'
                sh 'docker build -t arghyaroy123/node1:latest .'
                sh 'sudo -S chmod 777 name1.sh'
                sh 'sudo -S chmod 777 name2.sh'

                
            }
        }
         stage('docker login') {
            steps {
                withCredentials([string(credentialsId: 'doc1', variable: 'doc1')]) {  // create a secret text with variable name dockerhubpassword
                sh "docker login -u arghyaroy123 -p ${doc1}"     // if this job located in any folder in jenkins then it is a good practice create credential inside that folder                                                                          
                 }
            }
            }
        stage('push docker image') {
            steps {
                sh 'docker push arghyaroy123/node1:v1.$BUILD_ID'
                sh 'docker push arghyaroy123/node1:latest'
                sh 'docker image rmi arghyaroy123/node1:v1.$BUILD_ID'
                sh 'docker image rmi arghyaroy123/node1:latest'
            }
        }
        stage('run container in remote machine') {            
            steps {    
                sshagent(['node3'])  {
                sh "scp -o StrictHostKeyChecking=no *.sh ec2-user@172.31.13.106:."
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 ./name1.sh' // if my-app container running then it stop this and delete this
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 ./name2.sh" // it delete my-app container if available
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 docker run -d -p 3100:3100 --name my-app arghyaroy123/node1:latest"
                 }
                }
        }
    }
}
```
the dockerfile
```
FROM node:10-alpine

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install

COPY . .

CMD [ "node", "index.js" ]
```
name1.sh
```
#!/bin/bash
app="my-app"
if docker ps -a | grep "my-app";
then
docker stop "$app" && docker rm -f "$app"
else 
    echo "no container"
fi
```
name2.sh
```
#!/bin/bash
if  sudo docker images | grep "arghyaroy123/node1";
 then
docker image rm -f arghyaroy123/node1:latest
 else
 echo "no image"
fi
```
