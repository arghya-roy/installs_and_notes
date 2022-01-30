```
at first we need install nodejs in remote server and make the name.js as systemd
```
```
pipeline {
    agent any
  tools {
  nodejs 'nodejs'
}
    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/arghya-roy/node-app.git'
            }
        }
        stage('npm build') {
            steps {
                sh "npm install"
            }
        }
        
        stage('deploy') {
            steps {
                sshagent(['node3']) {
                sh 'scp -r * ec2-user@pvtip:/home/ec2-user/'  // node_modules is a folder so we have to copy recursively and * means all
                sh "ssh -o StrictHostKeyChecking=no ec2-user@pvtip sudo systemctl stop index"
                sh "ssh -o StrictHostKeyChecking=no ec2-user@pvtip sudo systemctl start index"
                
}
            }
        }
    }
}

```
