pipeline {
  agent any

  environment{
    CHECK_URL = "http://localhost:1912"
  }

  stages {

    stage('Start CPU') {
      options {
        timeout(time: 2, unit: "MINUTES")
      }

      steps {
        script {
          Exception caughtException = null

          catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') {
            try {
              sh '''export DD_PLATFORM=$PWD
              cd code/cpu/
              CURRENT_UID=$(id -u):$(id -g) MUID=$(id -u) docker-compose up -d
              curl -s --head --request GET localhost:1912 | grep 200'''
              return true
            } catch (Throwable e) {
              caughtException = e
            }
          }

          if (caughtException) {
            error caughtException.message
          }
        }
      }
    }

    stage('Start GPU') {
      options {
        timeout(time: 2, unit: "MINUTES")
      }

      steps {
        script {
          Exception caughtException = null

          catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') {
            try {
              sh '''export DD_PLATFORM=$PWD
              cd code/gpu/
              CURRENT_UID=$(id -u):$(id -g) MUID=$(id -u) docker-compose up -d
              curl -s --head --request GET localhost:1912 | grep 200'''
              return true
            } catch (Throwable e) {
              caughtException = e
            }
          }

          if (caughtException) {
            error caughtException.message
          }
        }
      }
    }

  }
}
