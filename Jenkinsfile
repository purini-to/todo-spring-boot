artifactId = null

pipeline {
  agent {
    docker {
      image 'gradle:jdk8'
    }
    
  }
  stages {
    stage('Promotion') {
      steps {
        inputArtifact()
        echoUserInput()
      }
    }
    stage('Compile') {
      steps {
        echoUserInput()
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

def inputArtifact() {
  artifactId = input(
    id: 'userInput', message: 'アーティファクトIDを入力してください。', parameters: [
    [$class: 'StringParameterDefinition', description: 'アーティファクトID', name: 'artifactId']
  ])
}

def echoUserInput() {
  echo artifactId
}
