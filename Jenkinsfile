pipeline {
  agent any 
    stages{
      stage("Docker Build"){
        steps{
          sh "docker build -t dockernginx ."   
        }  
      }
      stage("Run Docker image"){
        steps{
          sh "docker run --name nginx -itd -p 8082:80 dockernginx:latest"   
        }  
      }  
      stage("Pushing to docker hub"){
        steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub_credential', passwordVariable: 'pass', usernameVariable: 'userId')]) {
            sh 'docker login -u ${userId} -p ${pass}'
            sh "docker commit nginx kavi1997/docker:latest"
            sh "docker push kavi1997/docker:latest"   
          }
        }  
      }
   }
}
