
/* groovylint-disable-next-line CompileStatic */
pipeline
{
  agent any
  environment {
    registryCredential = 'Hub.Docker'
    containerName = 'testNode'
    String newVersion="8.8.8"
  }
  
  stages {
    stage('Build') {
      steps {
          script {
            dir ("${WORKSPACE}\\..\\GitHub.Testing") {
              echo "I am in : " + pwd()
              WORKSAPCE = pwd()
              echo 'new WORKSAPCE: ' + WORKSPACE

              if (fileExists('version.txt')) {
                newVersion = updateVersion()
                echo "Naya Version: " + newVersion
                writeFile file: 'version.txt', text: newVersion

                try {
                  withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'farrukhw_github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
                      bat("git config user.name ${env.GIT_USERNAME}")
                      bat("git config user.email 'farrukh1@gmail.com'")
                      bat("git commit version.txt -m \"Version updated to ${newVersion}\"")
                      bat("git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/Farrukhw/dice-hello-world-nodejs HEAD:master")
                      bat("git tag -f -a ${newVersion} -m \"Version updated to ${newVersion}\"")
                      bat("git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/Farrukhw/dice-hello-world-nodejs --tags --force")
                  }
                } finally {
                    bat("git config --unset user.name")
                    bat("git config --unset user.email")
                }
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


    post {
        always {
            script {
                if (currentBuild.currentResult == 'FAILURE') { // Other values: SUCCESS, UNSTABLE
                  buildName BUILD_NUMBER + '- ver: ' + newVersion + '- Failed'
                } else if (currentBuild.currentResult == 'UNSTABLE') {
                  buildName BUILD_NUMBER + '- ver: ' + newVersion + '- Unstable'
                } else {
                  buildName BUILD_NUMBER + '- ver: ' + newVersion
                }
                buildDescription "Git Branch: ${GIT_BRANCH} @ Node: ${NODE_NAME}"
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
