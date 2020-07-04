
/* groovylint-disable-next-line CompileStatic */
pipeline
{
  agent any
  environment {
    registryCredential = 'Hub.Docker'
    containerName = 'testNode'
    
  }
  stages {
    stage('Build') {
      steps {
          echo '============= testing ==================='
            script{
              WORKSAPCE="${WORKSAPCE}\\..\\GitHub.Testing"
            }
          echo "new WORKSAPCE: " + WORKSPACE
        }
      }
    }
}
