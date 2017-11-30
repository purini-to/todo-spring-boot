pipeline {
  agent {
    docker {
      image 'gradle:jdk8'
    }
    
  }
  stages {
    stage('Promotion') {
      steps {
        def userInput = input(id: 'userInput', message: 'Let\'s promote?')
        echo ("Env: "+userInput['env'])
        echo ("Target: "+userInput['target'])

        and the result will be
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
