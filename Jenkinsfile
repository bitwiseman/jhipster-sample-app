pipeline {
  agent {
    docker {
      image 'bitwiseman/training-blueocean-sample'
      args '-u root -v /home/jenkins/docker/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Backend": {
            sh './jenkins/test-backend.sh'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Frontend": {
            sh './jenkins/test-frontend.sh'
            junit '**/test-results/karma/*.xml'
            
          },
          "Static": {
            sh './jenkins/test-static.sh'
            
          },
          "Performance": {
            sh './jenkins/test-performance.sh'
            
          }
        )
      }
    }
    stage('Deploy to Staging') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
    stage('Deploy to Production') {
      steps {
        input(message: 'Deploy to staging?', ok: 'Fire away!')
        sh './jenkins/deploy.sh production'
        sh 'echo Notifying appropriate team members!'
        sh 'echo test'
      }
    }
  }
}