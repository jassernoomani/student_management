pipeline {
  agent any
  environment {
    SONAR_HOST = "http://172.18.40.239/:9000"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh "mvn -B -DskipTests clean verify"
      }
    }
    stage('SonarQube Analysis') {
      steps {
        withCredentials([string(credentialsId: 'jenkins-sonar', variable: 'jenkins-sonar')]) {
          sh "mvn -B sonar:sonar -Dsonar.host.url=${SONAR_HOST} -Dsonar.login=${jenkins-sonar}"
        }
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
    }
  }
}
