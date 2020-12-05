pipeline {
  agent any
    stages{
      stage("Docker Build"){
        steps{
           sh "set stage = ${env.STAGE_NAME}"
           sh "docker build -t dockernginx ."   
        }  
      }
      stage("Run Docker image"){
        steps{
           sh "set stage = ${env.STAGE_NAME}"
          sh "docker run --name nginx -itd -p 9090:80 dockernginx:latest"   
        }  
      }  
      stage("Pushing to docker hub"){
        steps{
           sh "set stage = ${env.STAGE_NAME}"
         withCredentials([usernamePassword(credentialsId: 'dockerhub_credential', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh 'docker login -u ${userId} -p ${pass}'
            sh 'docker commit nginx ${userId}/docker:latest'
            sh 'docker push ${userId}/docker:latest'   
          }
        }  
      }
     }  
  
      post { 
        always { 
           script{
        if (currentBuild.currentResult == "ABORTED" || currentBuild.currentResult == "FAILURE")
        {
          echo "The build is aborted or failed at ${stage} "
        }
             if (currentBuild.currentResult == "SUCCESS")
        {
            echo "The build is success "
        }
    }
    }
    }
   
}
