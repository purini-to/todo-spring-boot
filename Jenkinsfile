pipeline {
  agent {
    docker {
      image 'gradle:jdk8'
    }
    
  }
  stages {
    stage('Compile') {
      steps {
        sh 'gradle clean classes'
      }
    }
    stage('Test') {
      steps {
        sh 'gradle test'
        junit '**/build/test-results/test/TEST-*.xml'
      }
    }
    stage('Assemble') {
      steps {
        sh 'gradle assemble'
        stash(name: 'app', includes: '**/build/libs/*.war')
      }
    }
    stage('Promotion') {
      steps {
        timeout(time: 10, unit: 'MINUTES') {
          input 'Deploy to Production?'
        }
        
      }
    }
    stage('Deploy to Production') {
      steps {
        unstash 'app'
        sh 'ls -lt'
      }
    }
  }
  environment {
    TZ = 'Asia/Tokyo'
  }
}