pipeline {
  agent any
  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3-alpine'
          args '-v /home/bitwiseman/docker/.m2:/root/.m2'
        }
      }

      steps {
        sh 'mvn -B -DskipTests clean package'
        stash name: 'war', includes: 'target/**'
      }
    }
    stage('Backend') {
      steps {
        parallel(
          "Unit": {
            sh 'echo Unit'

          },
          "Performance": {
            sh 'echo Performance'

          }
        )
      }
    }
    stage('Frontend') {
      steps {
        sh 'echo Frontend'
      }
    }
    stage('Static Analysis') {
      steps {
        sh 'echo Static'
      }
    }
    stage('Deploy') {
      steps {
        sh 'echo Deploy'
      }
    }
  }
}
