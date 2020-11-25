pipeline {
  agent any
  stages {
    stage('Retrieve Sources') {
      steps {
        echo workspace
        git(branch: 'main', url: 'https://github.com/BRSweagle/Icarus')
      }
}
    stage('Config') {
      steps {
        SWEAGLEUpload(actionName: 'uploadProp', fileLocation: 'Components/Files/*.properties', format: 'properties', nodePath: 'Icarus', filenameNodes: true, withSnapshot: true, tag: '${BUILD_ID}')
        SWEAGLEUpload(actionName: 'uploadJSON', fileLocation: 'Components/Microservices/*.json', format: 'JSON', nodePath: 'Icarus', filenameNodes: true, withSnapshot: true, tag: '${BUILD_ID}')
        SWEAGLEUpload(actionName: 'uploadYAML', fileLocation: 'Components/Microservices/*.yaml', format: 'yaml', nodePath: 'Icarus', filenameNodes: true, withSnapshot: true, tag: '${BUILD_ID}')
        SWEAGLEUpload(actionName: 'uploadSpring', fileLocation: 'Components/Spring/*.properties', format: 'properties', nodePath: 'Icarus', filenameNodes: true, tag: '${BUILD_ID}', withSnapshot: true)
      }
    }

    stage('Test') {
      steps {
        echo 'Testing..'
        SWEAGLEValidate(actionName: 'Validate Tst-1', mdsName: 'Icarus.Tst-1', markFailed: true, warnMax: -1, noPending: true, stored: true)
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            echo 'Deploying....'
            SWEAGLESnapshot(actionName: 'snapshotDev', mdsName: 'Icarus.Dev-1', tag: '${BUILD_ID}')
            SWEAGLESnapshot(actionName: 'snapshotTST', mdsName: 'Icarus.Tst-1', tag: '${BUILD_ID}')
            SWEAGLESnapshot(actionName: 'snapshotProd', mdsName: 'Icarus.Prod-1', tag: '${BUILD_ID}')
          }
        }

        stage('SnowDevOps') {
          steps {
            echo 'Pushed results to ServiceNow DevOps'
          }
        }

      }
    }

    stage('End') {
      steps {
        echo 'End'
      }
    }

  }
}
