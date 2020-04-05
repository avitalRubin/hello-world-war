pipeline {

   

    agent any

    stages{

        stage('Build'){

            steps { 

                sh 'mvn clean package'
     
            }

            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts :  '**/target/*.war'

                }

            }

        }

       stage('sonarqube'){
          steps { 

                sh 'mvn verify sonar:sonar'
     
            }
           
        }
        
       stage(' Building Dockerfile'){
          steps { 

                sh  '''cp ${WORKSPACE}/target/hello-world-war-1.0.0.war ${WORKSPACE}
                                     docker build .''' 
     
            }
            
        }
       
       stage(' Tag Dockerfile'){
          steps { 

                sh  'docker tag $(docker images | grep \'<none>\' | head -n 1 | awk \'{print $3}\') java-app:${BUILD_ID}'
     
            }
            
        }
       
       stage(' Push image to Nexus'){
          steps {
            sh '''docker login -u admin -p avital-win 192.168.0.14:8083 
                                         docker tag java-app:${BUILD_ID} 192.168.0.14:8083/java-app:${BUILD_ID}
                                         docker push 192.168.0.14:8083/java-app:${BUILD_ID}
                                         docker rmi $(docker images --filter=reference="192.168.0.14:8083/java-app*" -q) -f
                                         docker logout 192.168.0.14'''
             
          }
       }
      

    }

}
