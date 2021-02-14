pipeline {
  agent any
  stages {

    stage('Cloning Project') {
      steps {
        git(url: 'https://github.com/bibah94/BiB-Devops.git', branch: 'master')
      }
    }

    stage('Build Project') {
      environment {
          PATH = "/opt/maven/bin:$PATH"
      }
      steps {
          sh "mvn clean install"
      }
    }

    stage('Static Code Analysis ') {
      environment{
        projectKey='BiB-Devops'
      }
      def scannerHome = tool 'sonarqube-scanner'
      steps {
        withSonarQubeEnv(envOnly: true, installationName: 'sonarqube-server', credentialsId: '4f92fd01-ca54-4b3d-b1fd-c96a30aa2e2a') {
    	    script {
            if(fileExists("sonar-project.properties")) {
              sh "${scannerHome}/bin/sonar-scanner"
            } else {
              sh '''${scannerHome}/bin/sonar-scanner     
                    -Dsonar.projectKey=${projectKey} 
                    -Dsonar.java.binaries=build/classes 
                    -Dsonar.java.libraries=**/*.jar 
                    -Dsonar.projectVersion=${BUILD_NUMBER}'''
            }
    	    }
        }		    
      }
    }
  }
}
