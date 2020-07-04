
/* groovylint-disable-next-line CompileStatic */
pipeline
{
  agent any
  environment {
    registryCredential = 'Hub.Docker'
    containerName = 'testNode'
    FARRUKHW_GITHUB_ID  = credentials('farrukhw_github')
    
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

                  withCredentials([usernameColonPassword(credentialsId: 'farrukhw_github', variable: 'FARRUKHW_GITHUB_ID')]) {
                    // bat "git commit version.txt -m \"Version updated to ${newVersion}\""
                    // bat 'git push'
                    echo FARRUKHW_GITHUB_ID
                  }

                  //git credentialsId: 'farrukhw_github', url: 'https://github.com/Farrukhw/dice-hello-world-nodejs'
             
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
