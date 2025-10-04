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

    stage('Execute Latest Build') {
      steps {
        script {
          // Find the latest built JAR file and execute it
          def jarFile = sh(script: "ls -t target/*.jar | head -n 1", returnStdout: true).trim()
          echo "Running latest build: ${jarFile}"
          sh "java -jar ${jarFile}"
        }
      }
    }
  }

  post {
    success {
      echo 'Build succeeded'
    }
    failure {
      echo 'Build failed'
    }
  }
}
