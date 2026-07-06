pipeline{
  agent any
  tools {
    nodejs 'node16'
  }
  environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
  stages{
      stage('Clean workspace'){
          steps{
              cleanWs()
          }
      }
      stage('Checkout from git'){
          steps{
              git branch: 'main' ,credentialsId: 'github-token', url: 'https://github.com/iam-rpoorna18/cicd-kubernetes-myapp.git'
           }
      }
      stage('Install Dependecy'){
          steps{
              sh "npm install"
           }
      }
      stage('Trivy FS Scan'){
          steps{
              sh "trivy fs . > trivy-log.txt"
           }
      }
      
  }









}
