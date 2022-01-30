```
pipeline {
    agent any
    environment {
  mvn = "/usr/bin/"
}

    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/arghya-roy/hello-world.git'
            }
        }
        stage('mvn build') {
            steps {
                sh 'mvn clean install package'
            }
        }
        stage('build docker image') {
            steps {
                sh 'docker build -t arghyaroy123/name1:v1.$BUILD_ID .'
                sh 'docker build -t arghyaroy123/name1:latest .'
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
                sh 'docker push arghyaroy123/name1:v1.$BUILD_ID'
                sh 'docker push arghyaroy123/name1:latest'
                sh 'docker image rmi arghyaroy123/name1:v1.$BUILD_ID'
                sh 'docker image rmi arghyaroy123/name1:latest'
            }
        }
        stage('run container in remote machine') {            
            steps {    
                sshagent(['tomcat4']) {
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.2.1 ./name1.sh"
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.2.1 ./name2.sh"
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.2.1 docker run -d -p 8080:8080 --name my-app arghyaroy123/name1:latest"
                 }
                }
        }
    }
}
```
