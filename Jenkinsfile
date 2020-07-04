
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
              WORKSAPCE = "${WORKSPACE}\\..\\GitHub.Testing"
            }
          echo 'new WORKSAPCE: ' + WORKSPACE
          dir (WORKSPACE) {
            String newVersion = updateVersion()
            
            writefile file: 'version.txt', text: newVersion
          }
        }
      }
    }
}



String updateVersion() {
    def orgVersion = readFile 'version.txt'
    def (major, minor, build) = orgVersion.tokenize('.').collect { it.toInteger() }
    build+=1
    if(build>=999) {
        minor+=1
        build=0
    }
    return "${major}.${minor}.${build}"
}