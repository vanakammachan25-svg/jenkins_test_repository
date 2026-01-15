pipeline {
    agent any
    
    tools{
        maven 'maven-3.9.6'
    }
    
    environment {
          IMAGE_NAME = 'deepakrj1999/deepakrj1999-sampleapplication'
          IMAGE_TAG  = '1.1'
    }

    stages {
        stage('Git Clone') {
            steps {
                //git 'https://github.com/vanakammachan25-svg/jenkins_test_repository.git'
                 git branch: 'main', url: 'https://github.com/vanakammachan25-svg/jenkins_test_repository.git'
            }
        }
        
        stage('Build'){
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker build'){
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }
        stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            docker push $IMAGE_NAME:$IMAGE_TAG
          '''
        }
      }
    }
    stage('Docker Container Run'){
        steps{
            sh '''
                  docker pull $IMAGE_NAME:$IMAGE_TAG || true
                
                  docker stop samplewebapp1 || true
                  docker rm samplewebapp1 || true
                
                  docker run -d \
                    --name samplewebapp1 \
                    -p 9090:8080 \
                    $IMAGE_NAME:$IMAGE_TAG
                '''
        }
    }
   }
}
