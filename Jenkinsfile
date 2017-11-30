pipeline {
  agent {
    docker {
      image 'gradle:jdk8'
    }
    
  }
  stages {
    stage('Promotion') {
      steps {
        def userInput = input(
          id: 'userInput', message: 'Let\'s go?', parameters: [
          [$class: 'TextParameterDefinition', defaultValue: 'a text\nwith several lines', description: 'A multiple lines text', name: 'aText'],
          [$class: 'StringParameterDefinition', defaultValue: 'a text', description: 'A String', name: 'aString'],
          [$class: 'BooleanParameterDefinition', defaultValue: true, description: 'A Boolean', name: 'aBoolean'],
          [$class: 'PasswordParameterDefinition', defaultValue: 'a password', description: 'A password', name: 'aPassword']
        ])
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
