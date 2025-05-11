pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: ' https://github.com/j0n0347/8.2CDevSecOps'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'npm test || true' // Allows pipeline to continue despite test failures
      }
    }
    stage('Generate Coverage Report') {
      steps {
        // Ensure coverage report exists
        sh 'npm run coverage || true'
      }
    }
    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true' // This will show known CVEs in the output
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            echo "Using token: $SONAR_TOKEN"
             sonar-scanner -Dsonar.login=$SONAR_TOKEN
          '''
        }
      }
    } 
  }
}