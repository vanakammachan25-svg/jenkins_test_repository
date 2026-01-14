pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Docker Build') {
      steps {
        sh 'docker build -t jenkins-demo:1.0 .'
      }
    }
  }
}
