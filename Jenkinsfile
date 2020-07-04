
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
          
              echo 'new WORKSAPCE: ' + WORKSPACE
              dir ("${WORKSPACE}\\..\\GitHub.Testing") {
                echo "I am in : " + pwd()
               
                if(fileExists('version.txt')) {
                  String newVersion = updateVersion()                
                  writefile file: 'version.txt', text: newVersion
                }
                else
                {
                  echo "No version.txt found in " + pwd()
                }
              }
            }
        }
      }
    }
}



String updateVersion() {
    echo "version.txt should be in " + pwd()
    def orgVersion = readFile 'version.txt'
    def (major, minor, build) = orgVersion.tokenize('.').collect { it.toInteger() }
    build+=1
    if(build>=999) {
        minor+=1
        build=0
    }
    return "${major}.${minor}.${build}"
}
