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
          }
        }

      }
    }

    stage('') {
      steps {
        archiveArtifacts 'app/build/libs/'
      }
    }

  }
}