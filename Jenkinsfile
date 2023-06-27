pipeline {
  agent any
  environment {
    registry = 'mvpar/devops20'
    registryCredential = 'dockerhub_id'
    dockerImage = ''
  }    
  stages {
    stage('Build Java Code') {
      steps {
        echo "${BUILD_NUMBER}"
        sh 'rm -f *.war && bash build.sh && mv ROOT.war ROOT${BUILD_NUMBER}.war'
      }
    }
    stage('upload artifacts to S3') {
      steps {
        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'devops93artifacts', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'devops-jen-profile', userMetadata: []
      }
    }
    stage('Build docker image') {
        steps {
            script {
              dockerImage = docker.build registry + ":$BUILD_NUMBER"
          }  
       }    
    }
    stage('Push docker image') {
        steps {
            script {
                docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                }
            }    
        }    
    }
  } 
}  
