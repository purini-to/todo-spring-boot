pipeline {
  agent {
    docker {
      image 'gradle:jdk8'
    }
    
  }
  stages {
    stage('Promotion') {
      steps {
        def a = input(message: 'Deploy to Production?', id: 'userInput', ok: 'yes', submitter: 'artifact', submitterParameter: 'artifact')
      }
    }
    stage('Compile') {
      steps {
        sh 'gradle clean classes'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            sh 'gradle test'
            junit '**/build/test-results/test/TEST-*.xml'
          }
        }
        stage('IntegrationTest') {
          steps {
            sh 'gradle integrationTest'
            junit '**/build/test-results/integrationTest/TEST-*.xml'
          }
        }
      }
    }
    stage('Assemble') {
      steps {
        sh 'gradle assemble'
        stash(name: 'app', includes: '**/build/libs/*.war')
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
