pipeline { 
    agent any
    
    environment {
        DOCKER_IMAGE="lbg"
        PORT=5001
    }
    stages {
        stage('Building image') {
            steps {
                sh '''
                sleep 3
                docker rm -f $(docker ps -aq) || true
                docker rmi -f $(docker images) || true
                docker build -t $DOCKER_IMAGE .
                '''
            }
        }
        stage('Running container') {
            steps {
                sh '''
                sleep 3
                docker run -d -p 5000:$PORT -e PORT=$PORT $DOCKER_IMAGE
                '''
           }
        }
        stage('Pushing image to dockerhub') {
            steps {
               sh '''
                  docker push davidfordj98/3839f9875827:latest 
                '''
             }
        }
    }
}
