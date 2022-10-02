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

    stage('') {
      steps {
        withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sqa_092669f06513ff63c8458bda3a1d3cebc3d725e4', envOnly: true) {
          sh 'mvn clean verify sonar:sonar'
        }

      }
    }

  }
}