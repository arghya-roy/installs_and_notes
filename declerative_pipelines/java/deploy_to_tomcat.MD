```
at the very first step we have to install tomcat in remote server
```
```
pipeline {
    agent any
    environment {
  mvn = "/usr/bin/"   // where mvn located in jenkins server
}
    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/arghya-roy/hello-world.git'
            }
        }
        stage('Maven build') {
            steps {
                sh "mvn clean install package"
            }
        }
        stage('deploy') {
            steps {
                sshagent(['tomcat4']) {  // before it have create credential in jenkins with ec2 username and private key 
                sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@<pvt_ip_of_tomcat_server>:/usr/share/tomcat/webapps/'
                
}
            }
        }
    }
}
```
