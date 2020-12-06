pipeline {
  agent any
    stages{
      stage("Docker Build"){
        steps{
           script {             
           last_started = env.STAGE_NAME
           sh "docker build -t dockernginx ."                        
        } 
        }
      }
      stage("Run Docker image"){
        steps{
          script {
           last_started = env.STAGE_NAME
          sh "docker run --name nginx -itd -p 9090:80 dockernginx:latest" 
            }
          }
        }  
       
      stage("Pushing to docker hub"){
        steps{
          script {           
          last_started = env.STAGE_NAME
         withCredentials([usernamePassword(credentialsId: 'dockerhub_credential', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh 'docker login -u ${userId} -p ${pass}'
            sh 'docker commit nginx ${userId}/docker:latest'
            sh 'docker push ${userId}/docker:latest'
      
            }                   
          }
        }  
      }
     }  
  
     
  post {
    success {
        echo 'Build is Success '    
    }
    failure {
      
       echo "Build failed at $last_started"
    }  
  }
   
}
