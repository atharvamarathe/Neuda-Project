pipeline {
  agent any
  stages {
    stage('Build Container') {
      steps {
        sh "docker-compose up"
      }
    }

    stage('Run tests against the container') {
      steps {
        sh 'curl http://localhost:8081/api/v1/employees'
      }
    }
    stage('Deploy Container To Openshift') {
      steps {
        sh "oc login https://localhost:8443 --username admin --password admin --insecure-skip-tls-verify=true"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        sh "oc expose svc/${projectName}"
      }
    }

  }

}