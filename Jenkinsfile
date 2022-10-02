pipeline {
  agent any
  stages {
    stage('checkout_code') {
      steps {
        cleanWs()
        git(url: 'https://github.com/sagishwartz/hello-world-war.git', branch: 'dev', credentialsId: 'github')
      }
    }

    stage('git_clone') {
      steps {
        sh 'git clone https://github.com/sagishwartz/infra-schwartz.git'
      }
    }

    stage('maven_build') {
      steps {
        sh 'mvn compile '
      }
    }

    stage('maven_test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('SQ') {
      steps {
         withSonarQubeEnv(installationName:'sonarqube') {
      sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sagishwartz_hello-world-war_AYOaCKmrGTlWuwPpLrza"
        }

      }
    }

  }
}
