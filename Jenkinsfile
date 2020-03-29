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
        
       stage('4. Building Dockerfile'){
          steps { 

                sh  '''cp ${WORKSPACE}/target/hello-world-war-1.0.0.war ${WORKSPACE}
                                     docker build .''' 
     
            }
            
        }

    }

}
