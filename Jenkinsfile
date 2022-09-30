pipeline {
  agent any
  stages {
    stage('checkout_code') {
      steps {
        git(url: 'https://github.com/sagishwartz/hello-world-war.git', branch: 'dev', credentialsId: 'github2')
      }
    }

    stage('git_clone') {
      steps {
        sh 'git clone https://github.com/sagishwartz/infra-schwartz.git'
      }
    }

  }
}