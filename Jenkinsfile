pipeline {
  agent any 
    stages{
      try{
      stage("Docker Build"){
        steps{
          sh "docker build -t dockernginx ."   
        }  
      }
      stage("Run Docker image"){
        steps{
          sh "docker run --name nginx -itd -p 9090:80 dockernginx:latest"   
        }  
      }  
      stage("Pushing to docker hub"){
        steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub_credential', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh 'docker login -u ${userId} -p ${pass}'
            sh 'docker commit nginx ${userId}/docker:latest'
            sh 'docker push ${userId}/docker:latest'   
          }
        }  
      }
     }  
      catch (Exception err)
           {
                echo "Error caught${err}"
                failedStage = "${pipelineStage}"
                echo "Build failed at ${pipelineStage}"
           }
   }
}
