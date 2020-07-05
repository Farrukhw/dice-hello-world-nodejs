
/* groovylint-disable-next-line CompileStatic */
pipeline
{
  agent any
  environment {
    registryCredential = 'Hub.Docker'
    containerName = 'testNode'
    //FARRUKHW_GITHUB_ID  = credentials('farrukhw_github')
    
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

                  try {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'farrukhw_github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
                      bat("git config credential.username ${env.GIT_USERNAME}")
                      bat("${git} config credential.helper '!echo password=\$GIT_PASSWORD; echo'")
                      bat("git commit version.txt -m \"Version updated to ${newVersion}\"")
                      bat('GIT_ASKPASS=true git push origin')
                      bat("git tag -f -a ${newVersion} -m \"Version updated to ${newVersion}\"")
                      bat('GIT_ASKPASS=true git push origin --tags --force')
                    }
                  } finally {
                      bat('git config --unset credential.username')
                      bat('git config --unset credential.helper')
                  }


                  // withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'farrukhw_github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
                  //     bat("git config credential.username ${env.GIT_USERNAME}")
                  //     bat("git config credential.helper '!echo password=\$GIT_PASSWORD; echo'")
                  //     bat("git config user.name 'farrukh'")
                  //     bat("git config user.email 'farrukh1@gmail.com'")
                  //     bat("git commit version.txt -m \"Version updated to ${newVersion}\"")
                  //     bat("git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/Farrukhw/dice-hello-world-nodejs HEAD:master")
                  //     bat("git tag -f -a ${newVersion} -m \"Version updated to ${newVersion}\"")
                  //     bat("git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/Farrukhw/dice-hello-world-nodejs --tags --force")
                  // }
                  // buildName BUILD_NUMBER + ' - ver: ' + newVersion
                  // buildDescription "Git Branch: ${GIT_BRANCH} @ Node: ${NODE_NAME}"
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
    echo "Current Version: ${orgVersion}"
    def (major, minor, build) = orgVersion.tokenize('.').collect { it.toInteger() }
    build+=1
    if(build>=999) {
        minor+=1
        build=0
    }
    echo "Newly calculated Version: ${major}.${minor}.${build}"
    return "${major}.${minor}.${build}"
}
