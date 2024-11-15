pipeline { 
    agent any
    
    environment {
        DOCKER_IMAGE="lbg"
        PORT="5001"
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
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDENTIALS', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh '''
                        docker tag $DOCKER_IMAGE $DOCKER_USERNAME/$DOCKER_IMAGE
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push ${DOCKER_USERNAME}/$DOCKER_IMAGE:latest 
                        '''
              }    
           }
         }
      }
   }
}
