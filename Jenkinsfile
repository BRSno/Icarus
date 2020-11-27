pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(url: 'https://github.com/BRSno/Percy', branch: 'main', poll: true)
      }
    }

    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            SWEAGLEUpload(actionName: 'uploadProp', fileLocation: 'Components/Files/*.properties', format: 'properties', nodePath: 'Icarus', tag: '${BUILD_ID}', withSnapshot: true, filenameNodes: true)
            SWEAGLEUpload(actionName: 'uploadJSON', fileLocation: 'Components/Microservices/*.json', format: 'JSON', nodePath: 'Icarus', filenameNodes: true, tag: '${BUILD_ID}', withSnapshot: true)
            SWEAGLEUpload(actionName: 'uploadini', fileLocation: 'config.ini', format: 'ini', nodePath: 'Icarus,Components,Files', filenameNodes: true, tag: '${BUILD_ID}', withSnapshot: true)
          }
        }

        stage('Validate') {
          steps {
            SWEAGLEValidate(actionName: 'NoHTTP', mdsName: 'Icarus', stored: true)
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        sleep 3
        echo 'Deploy complete'
      }
    }

  }
}