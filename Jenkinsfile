pipeline{  
    environment {
    registry = "deepak2910/capstone"
    }
  agent any
  stages {
      
           stage('Sonarqube-anaysis') {
          
           steps {
                
                   sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=springboot-app \
  -Dsonar.host.url=http://34.173.221.228:9000 \
  -Dsonar.login=sqp_017213ba9b23e79eb4e5a32ea96e2a3c6244a993\
  -Dcheckstyle.skip'
                   
                      }
       }
      
      
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
                   sh "kubectl rollout status deployment springboot"
                   sh "kubectl get service springboot-svc"
                }
           }
       }
   }
}



