pipeline {
  agent any
  stages {
    stage('Build Java Code') {
      steps {
        echo "${BUILD_NUMBER}"
        sh 'bash build.sh'
      }
    }
  }
}
