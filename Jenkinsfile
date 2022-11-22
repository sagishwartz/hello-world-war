pipeline {
  environment {
        registry = "sagishwartz/final"
        registryCredential = 'docker_hub'
    }
    tools {
       terraform 'terraform'
    }
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
        dir(path: 'infra-schwartz') {
          sh 'git checkout dev'
          sh 'cp Dockerfile /var/lib/jenkins/workspace/final/'
        }

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
        withSonarQubeEnv('SQ') {
          sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=final_project'
        }

      }
    }

    stage('docker_build_tag') {
      steps {
        sh 'docker build -t sagishwartz/final:$BUILD_NUMBER .'
      }
    }

    stage('push_image') {
        steps {
            script {
                docker.withRegistry( '', registryCredential ) {
                    sh "docker push sagishwartz/final:$BUILD_NUMBER"
                }
            }
        }
    }
    stage('terraform format check') {
        steps{
            sh 'sh cd /var/lib/jenkins/workspace/final/infra-schwartz'
            sh 'terraform fmt'
            }
        }
    stage('terraform Init') {
        steps{
            sh 'terraform init'
            }
        }
    stage('terraform apply') {
        steps{
            sh 'terraform apply --auto-approve'
            }
        }
  }
}
