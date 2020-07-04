
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
              dir ("${WORKSPACE}\\..\\GitHub.Testing") {
                echo "I am in : " + pwd()
                WORKSAPCE = pwd()
                echo 'new WORKSAPCE: ' + WORKSPACE                
                if(fileExists('version.txt')) {
                  String newVersion = updateVersion()                
                  echo "Naya Version: " + newVersion
                  writeFile file: 'version.txt', text: newVersion
                  bat "git commit version.txt -m \'Version updated to ${newVersion}\'"
                  bat 'git push'
                }
                else
                {
                  echo 'No version.txt found in ' + pwd()
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
