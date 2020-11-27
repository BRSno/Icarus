pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(url: 'https://github.com/BRSno/Percy', branch: 'main', poll: true)
      }
    }

    stage('Test') {
      steps {
        SWEAGLEUpload(actionName: 'uploadProp', fileLocation: 'Components/Files/*.properties', format: 'properties', nodePath: 'Icarus', tag: '${BUILD_ID}', filenameNodes: true)
        SWEAGLEUpload(actionName: 'uploadJSON', fileLocation: 'Components/Microservices/*.json', format: 'JSON', nodePath: 'Icarus', filenameNodes: true, tag: '${BUILD_ID}')
        SWEAGLEUpload(actionName: 'uploadini', fileLocation: '*', format: 'ini', nodePath: 'Icarus,Components,Files', filenameNodes: true, tag: '${BUILD_ID}')
        SWEAGLEValidate(actionName: 'NoHTTP', mdsName: 'Icarus', stored: true, errMax: -1)
      }
    }

    stage('Deploy') {
      steps {
        sleep 3
        echo 'Deploy complete'
        SWEAGLESnapshot(actionName: 'Snapshot', mdsName: 'Icarcu', tag: '${BUILD_ID}')
      }
    }

  }
}