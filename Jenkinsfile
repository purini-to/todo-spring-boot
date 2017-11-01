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
  }
  environment {
    TZ = 'Asia/Tokyo'
  }
}