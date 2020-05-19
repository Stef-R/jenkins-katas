pipeline {
  agent any
  stages {
    stage('Parallel execution') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "Hallo verden"'
          }
        }

        stage('build app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh '''environment {
      DOCKERCREDS = credentials(\'docker_login\') //use the credentials just created in this stage
}
steps {
      unstash \'code\' //unstash the repository code
      sh \'ci/build-docker.sh\'
      sh \'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin\' //login to docker hub with the credentials above
      sh \'ci/push-docker.sh\'
}'''
            }
          }

        }
      }

    }
  }