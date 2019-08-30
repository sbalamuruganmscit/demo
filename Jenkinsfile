pipeline {

  agent any
  tools {
        maven 'M3'
   }
  stages {
        stage ('Checkout') {
            steps {
                checkout scm
             }
        }
        stage ('Build') {
            steps {            
              sh "mvn  clean install -DskipTests=true"
            }
        }
         stage ('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage ('Build Docker Image') {
            steps {
                sh "docker version"
                sh "docker build -t barathece91/demo-jenkins ."
            }
        }
        
        stage ('Deploy Docker Image') {
            steps {  
                sh '''
                    echo stop the running containters
                    CONTAINER_NAME='demo-app'
                    echo container name $CONTAINER_NAME
                    CID=$(docker ps -q -f status=running -f name=^/${CONTAINER_NAME}$)
                    if [ ! "${CID}" ]; then
                      echo "Container doesn't exist"
                    fi
                    docker stop demo-app && docker rm demo-app
                    echo existing containers are stopped
                    docker run --name demo-app -p 8081:8080 -d barathece91/demo-jenkins
                  '''
            }
        }
    }

}
