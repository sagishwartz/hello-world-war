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
      stage('checkout_code_Terraform') {
      steps {
        cleanWs()
        git(url: 'https://github.com/sagishwartz/terraform-provision-eks-cluster.git', branch: 'main', credentialsId: 'github')
      }
    }
    stage('terraform format check') {
        steps{
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
    stage('Deploy hellowrold') {
        steps{
            script{
                cluster_name = sh (script: "terraform output cluster_name", returnStdout: true).trim()
                region = sh (script: "terraform output region", returnStdout: true).trim()
                sh 'aws eks update-kubeconfig --region ${region} --name ${cluster_name}'
                sh 'kubectl apply -f helloworld_deployment.yml'
            }
            }
        }
  }
}
