pipeline {
  agent {
    docker {
      image 'java:8-jdk-alpine'
    }
    
  }
  stages {    
    stage('コンパイル') {
      steps {
        sh './gradlew clean classes'
      }
    }
    stage('テスト') {
      parallel {
        stage('ユニットテスト') {
          steps {
            sh './gradlew test'
          }
          post {
            always {
              junit '**/build/test-results/test/TEST-*.xml'
            }
          }
        }
        stage('インテグレーションテスト') {
          steps {
            sh './gradlew integrationTest'
          }
          post {
            always {
              junit '**/build/test-results/integrationTest/TEST-*.xml'
            }
          }
        }
      }
    }
    stage('ビルド') {
      steps {
        sh './gradlew assemble'
        stash(name: 'app', includes: '**/build/libs/*.war')
      }
    }
    stage('デプロイ確認(10分以内)') {
      steps {
        timeout(time: 10, unit: 'MINUTES') {
          input 'Deploy to Production?'
        }
      }
    }
    stage('本番デプロイ') {
      steps {
        unstash 'app'
        sh 'echo "Deploy"'
      }
    }
  }
  environment {
    TZ = 'Asia/Tokyo'
  }
}
