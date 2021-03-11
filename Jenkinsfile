pipeline {
  agent none
  environment {
    PATH = "/opt/maven/bin:$PATH"
    scannerHome = tool 'sonar-scanner'
  }
  stages {

    stage('Cloning Project') {
      steps {
        git(url: 'https://github.com/bibah94/BiB-Devops.git', branch: 'master')
      }
    }

    stage('Build & Static Code Analysis') {
      agent any
      steps {
        withSonarQubeEnv(envOnly: true, installationName: 'sonarqube-server', credentialsId: '4f92fd01-ca54-4b3d-b1fd-c96a30aa2e2a') {
          sh "mvn clean package sonar:sonar"
        }       
      }
    }

    stage('Quality Gate') {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}