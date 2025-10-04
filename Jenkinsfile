pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        // run maven installed inside the Jenkins container
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Unit tests') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
  post {
    success { echo 'Build succeeded' }
    failure { echo 'Build failed' }
  }
}
