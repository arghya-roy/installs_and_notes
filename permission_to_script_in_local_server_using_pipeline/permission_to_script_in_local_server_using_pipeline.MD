
permission_to_script_in_local_server_using_pipeline

 ```stage('build docker image') {
            steps {
                sh 'sudo -S chmod 777 path/to/script/name.sh'
            }```
