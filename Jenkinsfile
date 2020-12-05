pipeline {
  agent any
    stages{
      stage("Docker Build"){
        steps{
           script {
             try{
                    last_started = env.STAGE_NAME
           sh "docker build -t dockernginx ."  
             }
             catch (err) {
                echo err.getMessage()
            }
        } 
        }
      }
      stage("Run Docker image"){
        steps{
          script {
            try{
                    last_started = env.STAGE_NAME
          sh "docker run --name nginx -itd -p 9090:80 dockernginx:latest" 
            }
              catch (err) {
                echo err.getMessage()
            }
          }
        }  
      }  
      stage("Pushing to docker hub"){
        steps{
          script {
            try{
                    last_started = env.STAGE_NAME
         withCredentials([usernamePassword(credentialsId: 'dockerhub_credential', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh 'docker login -u ${userId} -p ${pass}'
            sh 'docker commit nginx ${userId}/docker:latest'
            sh 'docker push ${userId}/docker:latest'
         }
            }
           catch (err) {
                echo err.getMessage()
            }
         }
          }
        }  
      }
     }  
  
     
         post {
    success {
        echo 'Successfully completed '    
    }
    failure {
      
       echo "Build failed at $last_started due to $err"
    }  
  }
   
}
