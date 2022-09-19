pipeline{  
    environment {
    registry = "https://hub.docker.com/repository/docker/deepak2910"
    }
  agent any
  stages {
           stage('Build') {
          
           steps {
                
                   sh 'pwd'
                   sh 'docker build .'
                      }
       }
       
      
       stage('Publish') {
           environment {
               registryCredential = 'docker-login'
           }
           steps{
              
              script {
                 
                  docker.withRegistry( '', registryCredential ) {
                      def appimage = docker.build registry + ":$BUILD_NUMBER"
                      appimage.push()
                      
                  }
              }
           }
       }
       stage ('Deploy') {
           steps {
               script{
                   def image_id = registry + ":$BUILD_NUMBER"
                   sh "sed -i 's|image_id|$image_id|g' deployment.yml"
                   sh "kubectl apply -f deployment.yml -f service.yml"
                   sh "kubectl rollout status deployment hello-deployment"
                   sh "kubectl get service hello-svc"
                }
           }
       }
   }
}



