pipeline {
  agent any

  environment {
    NETLIFY_SITE_ID = '3cd2f6d4-089e-4a13-893c-588a453810f8'
    NETLIFY_AUTH_TOKEN = credentials('NETLIFY_AUTH_TOKEN')
  }

  stages {
    stage('Build') {
      agent {
        docker {
          image 'node:25-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          ls -la
          node --version
          npm --version
          npm ci
          npm run build
          ls -la
        '''
      }
    }
    stage('Test') {
      agent {
        docker {
          image 'node:25-alpine'
          reuseNode true
        }
      }

      steps {
        sh '''
          test -f build/index.html
          npm test
        '''
      }
    }

    stage('Deploy') {
      agent {
        docker {
          image 'node:25-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          npm install netlify-cli --save-dev
          npx netlify --version
          echo "deploying to prod, id: $NETLIFY_SITE_ID"
          npx netlify status
          npx netlify deploy --dir=build --prod
        '''
      }
    }
  }

  post {
    always {
      junit 'test-results/junit.xml'
    }
  }
}
