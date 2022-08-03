pipeline {
    agent any

    stages {
        stage ('Build Stage') {
            steps {
            	echo 'Project build started.'
    			sh './gradlew build'
  				echo 'Project build finished.'
            }
        }
        stage ('Auth Token Stage') {
            steps {
           			sh 'export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 access-key-id AKIARYP3FTTYZXSU6KF3 secret-access-key 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 --region ap-south-1 --query authorizationToken --output text`'
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