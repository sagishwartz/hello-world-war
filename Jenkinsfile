pipeline {
  agent none
  stages {
    stage('checkout_code') {
      steps {
        git(url: 'https://github.com/sagishwartz/hello-world-war.git', branch: 'dev', changelog: true, credentialsId: 'github_cred', poll: true)
      }
    }

    stage('') {
      steps {
        sh 'git clone https://github.com/sagishwartz/infra-schwartz.git'
      }
    }

  }
}