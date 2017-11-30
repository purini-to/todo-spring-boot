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
    stage('Promotion') {
      def userInput = input(
       id: 'userInput', message: 'Let\'s promote?', parameters: [
       [$class: 'TextParameterDefinition', defaultValue: 'uat', description: 'Environment', name: 'env']
      ])
      echo ("Env: "+userInput)
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
