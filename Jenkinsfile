pipeline {
  agent {
    docker {
      image 'gradle:jdk8'
    }
    
  }
  stages {    
    stage('コンパイル') {
      steps {
        sh 'gradle clean classes'
      }
    }
    stage('テスト') {
      parallel {
        stage('ユニットテスト') {
          steps {
            sh 'gradle test'
            junit '**/build/test-results/test/TEST-*.xml'
          }
        }
        stage('インテグレーションテスト') {
          steps {
            sh 'gradle integrationTest'
            junit '**/build/test-results/integrationTest/TEST-*.xml'
          }
        }
      }
    }
    stage('ビルド') {
      steps {
        sh 'gradle assemble'
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
        sh 'ls -lt'
      }
    }
  }
  environment {
    TZ = 'Asia/Tokyo'
  }
}
