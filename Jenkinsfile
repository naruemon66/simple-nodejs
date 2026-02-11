pipeline {
  environment {
    VERCEL_PROJECT_NAME = 'simple-nodejs'
    VERCEL_TOKEN = credentials('devops15-vercel-token')
  }
  agent {
    kubernetes {
      yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: my-builder
    image: node:20-alpine
    command:
    - cat
    tty: true
'''
    }
  }
  stages {
    stage('Check Node') {
      steps {
        container('my-builder') {
          sh 'node -v'
          sh 'npm -v'
        }
      }
    }

    stage('Install Dependencies') {
      steps {
        container('my-builder') {
          sh 'npm ci'
        }
      }
    }

    stage('Deploy to Vercel') {
      steps {
        container('my-builder') {
          sh 'npm install -g vercel@latest'
          sh '''
            vercel link --project $VERCEL_PROJECT_NAME --token $VERCEL_TOKEN --yes
            vercel --token $VERCEL_TOKEN --prod --confirm
          '''
        }
      }
    }
  }
}
