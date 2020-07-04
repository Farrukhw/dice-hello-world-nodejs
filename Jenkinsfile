
/* groovylint-disable-next-line CompileStatic */
pipeline
{
  agent any
  environment {
    registryCredential = 'Hub.Docker'
    containerName = 'testNode'
    WORKSAPCE="${WORKSAPCE}\\..\\GitHub.Testing"
  }
  stages {
    stage('Build') {
      steps {
          echo '============= testing ==================='
          echo 'WORKSAPCE: ' + WORKSAPCE
        }
      }
    }
}
