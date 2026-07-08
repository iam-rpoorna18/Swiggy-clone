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
             withDockerRegistry(url:'https://index.docker.io/v1/', credentialsId: 'dockerhub-id') {
                sh "docker info | grep Username || true"
                sh "docker build -t swiggy-clone:${IMAGE_TAG} . "
                sh "docker tag swiggy-clone:${IMAGE_TAG} pkumarr/swiggy-clone:${IMAGE_TAG}"
                sh "docker push pkumarr/swiggy-clone:${IMAGE_TAG}"
              }
           }
      }
      stage('Trivy image Scan'){
          steps{
              sh "trivy image pkumarr/swiggy-clone:${IMAGE_TAG}  > trivy-image-log.txt"
           }
      }
      stage('Deploy to k8s'){
            steps{
                script{
                   sh 'kubectl apply -f Kubernetes/deployment.yml'
                   sh 'kubectl set image deployment/swiggy-app swiggy-app=pkumarr/swiggy-clone:${IMAGE_TAG}'
                   sh 'kubectl apply -f Kubernetes/service.yml'
                }
            }
        }
        stage('Verify') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
                sh 'kubectl get ingress'
            }
        }
      
  }

}
