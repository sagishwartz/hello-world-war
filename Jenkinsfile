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
      
     stage('build_war') {
       mvn compile
   }

   
     stage('test_war') {
       mvn test
   }
   }

  }
}
