def projectName = 'jenkins-project'
def version = "0.0.1"
def dockerImageTag = "project_service"
pipeline {
  agent any
  stages {
    stage('Build Container') {
      steps {
        sh "docker-compose build"
      }
    }

    stage('Run tests against the container') {
      
        parallel {
          stage('run') {
            options {
                timeout(time: 30, unit: "SECONDS")
            }
            steps {
              sh 'docker-compose up'
            }
          }
          stage('test') {
            steps {
              sleep(10)
              sh 'curl http://localhost:8081/api/v1/employees'
              sh 'curl http://localhost:9091'
            }
          }
        }
    }
  }

}