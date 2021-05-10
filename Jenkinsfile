pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(url: 'https://github.com/BRSno/Icarus', branch: 'main', poll: true)
        snDevOpsStep(enabled:true)
      }
    }

    stage('Test') {
      steps {
        SWEAGLEUpload(actionName: 'uploadProp', fileLocation: 'Components/Files/*.properties', format: 'properties', nodePath: 'Icarus', tag: '${BUILD_ID}', filenameNodes: true)
        SWEAGLEUpload(actionName: 'uploadJSON', fileLocation: 'Components/Microservices/*.json', format: 'JSON', nodePath: 'Icarus', filenameNodes: true, tag: '${BUILD_ID}')
        SWEAGLEUpload(actionName: 'uploadprops', fileLocation: 'Components/Files/*.properties', format: 'properties', nodePath: 'Icarus,Components,Files', filenameNodes: true, tag: '${BUILD_ID}')
        SWEAGLEValidate(actionName: 'NoHTTP', mdsName: 'Icarus', warnMax: 1, markFailed: true)
      }
    }

    stage('Deploy') {
      steps {
        sleep 3
        echo 'Deploy complete'
        SWEAGLESnapshot(actionName: 'Snapshot', mdsName: 'Icarus', tag: '${BUILD_ID}')
        snDevOpsChange()
      }
    }

  }
}
