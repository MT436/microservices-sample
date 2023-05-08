pipeline {     
   agent {
     label {
     label 'slave1'
     retries 3
        }
    }    
         stage("build"){
             steps{
                 sh 'mvn clean package'
             }
         }
         
         stage("sonar"){
             steps{
                   mvn clean verify sonar:sonar \
                   -Dsonar.projectKey=microservices \
                   -Dsonar.projectName='microservices' \
                   -Dsonar.host.url=http://65.0.68.145:9000 \
                   -Dsonar.token=sqp_4f5bfb7c773a26c072e10d039a498ca5f29c5a79
             }
         }
          stage('Publish') {
            steps {
                // Publish artifacts to Artifactory
                rtUpload (
                    serverId: 'jfrog', // ID of the Artifactory server configured in Jenkins
                    spec: '''{
                        "files": [{
                            "pattern": "target/*.war",
                            "target": "http://172.31.18.18:8082/artifactory/microservices/"
                        }]
                    }'''
                )
            }
        }
        stage("deploy"){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://172.31.18.18:8085/')], contextPath: '/new/', war: '**/*.war'
            }
        }
         
         
     }
}
