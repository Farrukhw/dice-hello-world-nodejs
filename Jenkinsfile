def receiver_container
def dockerImage
pipeline { 
  agent any 
  environment { 
    registryCredential = 'Hub.Docker' 
  } 
  stages { 
    stage('Build') { 
      steps { 

        sh 'docker container rm -f testNode || true' 
        script {
            dockerImage=docker.build("farrukhw/test-node-app:latest")
        }

      } 
    } 
    stage('Test') { 
      steps { 
        script {
            receiver_container=dockerImage.run("-d -p 8001:80080 --name='testNode'")
        }
        // sh 'docker container rm -f node || true'  
        // sh 'docker container run -p 8001:8080 --name node -d farrukhw/test-node-app' 
        sh 'curl -I http://localhost:8001' 
      } 
    } 
    stage('Publish') { 
        steps{ 
            script { 
                docker.withRegistry( '', registryCredential ) { 
                  //sh 'docker push farrukhw/test-node-app:latest' 
                  dockerImage.push()
                } 
            } 
        } 
      } 
    } 

    post {
      always {
        script {
          receiver_container.stop()
          receiver_container.remove()
          
        }
      }
    }
}
