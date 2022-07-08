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

    // stage('Run tests against the container') {
    //   steps {
    //     sh 'curl http://localhost:8081/api/v1/employees'
    //   }
    // }
    stage('Deploy Container To Openshift') {
      steps {
        sh "oc login https://localhost:8080 --username admin --password c0nygre --insecure-skip-tls-verify=true"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        sh "oc expose svc/${projectName}"
      }
    }

  }

}