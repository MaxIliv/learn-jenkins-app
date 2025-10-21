pipeline {
    agent any
    stages {
        stage('w/o docker') {
      steps {
        sh '''
            echo "without docker"
            ls -la
            touch containet-no.txt
        '''
      }
        }

        stage('w/ docker') {
      agent {
        docker {
          image 'node:25-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
            echo "with docker"
            npm --version
            ls -la
            touch containet-yes.txt
        '''
      }
    }
  }
}
