pipeline {
    agent any

    stages {
        stage ('Build Stage') {
            steps {
            	echo 'Project build started.'
           			sh 'export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 --query authorizationToken --output text`'
    				sh './gradlew build'
  				
  				echo 'Project build finished.'
            }
        }
        stage ('Publish Stage') {
            steps {
            	echo 'Publishing dependencies to AWS CodeArtifact started.'
           		withGradle {
    				sh './gradlew publish'
  				}
  				echo 'Publishing dependencies to AWS CodeArtifact finished.'
            }
        }
    }
}