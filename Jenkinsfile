
/* groovylint-disable-next-line CompileStatic */
pipeline
{
  agent any
  environment {
    registryCredential = 'Hub.Docker'
    containerName = 'testNode'
  }

  String newVersion = '8.8.8'
  String versionFile = 'version.txt'
  stages {
    stage('Build') {
      steps {
          script {
            dir ("${WORKSPACE}\\..\\GitHub.Testing") {
              echo 'I am in : ' + pwd()
              WORKSAPCE = pwd()
              echo 'new WORKSAPCE: ' + WORKSPACE

              /* groovylint-disable-next-line NestedBlockDepth */
              if (fileExists(versionFile)) {
                newVersion = updateVersion()
                echo 'Naya Version: ' + newVersion
                writeFile file: versionFile, text: newVersion

                /* groovylint-disable-next-line LineLength, NestedBlockDepth */
                try {
                  /* groovylint-disable-next-line NestedBlockDepth */
                  withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'farrukhw_github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
                      bat("git config user.name ${env.GIT_USERNAME}")
                      bat("git config user.email 'farrukh1@gmail.com'")
                      bat("git commit version.txt -m \"Version updated to ${newVersion}\"")
                      bat("git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/Farrukhw/dice-hello-world-nodejs HEAD:master")
                      bat("git tag -f -a ${newVersion} -m \"Version updated to ${newVersion}\"")
                      bat("git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/Farrukhw/dice-hello-world-nodejs --tags --force")
                  }
                } finally {
                    bat('git config --unset user.name')
                    bat('git config --unset user.email')
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
                  buildName BUILD_NUMBER + ' - ver: ' + newVersion + ' - Failed'
                } else if (currentBuild.currentResult == 'UNSTABLE') {
                  buildName BUILD_NUMBER + ' - ver: ' + newVersion + ' - Unstable'
                } else {
                  buildName BUILD_NUMBER + ' - ver: ' + newVersion
                }
                buildDescription "Git Branch: ${GIT_BRANCH} @ Node: ${NODE_NAME}"
            }
        }
    }
}

String updateVersion() {
    String orgVersion = readFile versionFile
    echo "Current Version: ${orgVersion}"
    /* groovylint-disable-next-line NoDef */
    def (major, minor, build) = orgVersion.tokenize('.').collect { it.toInteger() }
    build += 1
    if (build >= 999) {
        minor += 1
        build = 0
    }
    echo "Newly calculated Version: ${major}.${minor}.${build}"
    return "${major}.${minor}.${build}"
}
