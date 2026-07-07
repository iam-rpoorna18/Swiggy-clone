pipeline{
  agent any
  tools {
    nodejs 'node16'
  }
  environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
  stages{
  //    stage('Clean workspace'){
   //       steps{
    //          cleanWs()
    //      }
    //  }
      stage('Debug') {
            steps {
                sh '''
                  pwd
                  ls -ltr
                  find .
                '''
            }
      }
      //stage('Checkout from git'){
      //    steps{
      //        git branch: 'main' ,credentialsId: 'github-token', url: 'https://github.com/iam-rpoorna18/Swiggy-clone.git'
       //    }
      //}
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
      stage('Docker Build and Push'){
          steps{
             withDockerRegistry(url:'https://registry-1.docker.io', credentialsId: 'dockerhub-id') {
                sh "docker build -t swiggy-clone:${IMAGE_TAG} . "
                sh "docker tag swiggy-clone:${IMAGE_TAG} pkumarr/swiggey-clone:${IMAGE_TAG}"
                sh "docker push pkumarr/swiggey-clone:${IMAGE_TAG}"
              }
           }
      }    
      
  }

}
