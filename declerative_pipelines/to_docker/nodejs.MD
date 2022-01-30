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
        stage('npm build') {
            steps {
                sh 'npm install'
            }
        }
        stage('build docker image') {
            steps {
                sh 'docker build -t arghyaroy123/node1:2.0 .'
                sh 'docker build -t arghyaroy123/node1:latest .'
                // sh 'sudo -S chmod 777 name.sh'
                
            }
        }
         stage('docker login') {
            steps {
                sh "docker login -u arghyaroy123 -p arghyaroy123"
                 }
            }
        stage('push docker image') {
            steps {
                sh 'docker push arghyaroy123/node1:2.0'
                sh 'docker push arghyaroy123/node1:latest'
            }
        }
        stage('run container in remote machine') {            
            steps {    
                sshagent(['node3'])  {
                sh "scp -o StrictHostKeyChecking=no *.sh ec2-user@172.31.13.106:."
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 ./name1.sh' // if my-app container running then it stop this
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 ./name2.sh" // it delete my-app container if available
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 ./name3.sh" // it deletes arghyaroy123:latest img if available
                sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.13.106 docker run -d -p 3100:3100 --name my-app arghyaroy123/node1:latest"
                 }
                }
        }
    }
}
```
