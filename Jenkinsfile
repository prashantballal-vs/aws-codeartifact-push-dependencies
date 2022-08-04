pipeline {
    agent any
    
    environment {
    	CURRENT_BUILD_DISPLAY="0.1.${BUILD_NUMBER}"
    	PROJECT_FOLDER="."
    	AWS_ACCESS_KEY_ID = sh 'export AWS_ACCESS_KEY_ID=AKIARYP3FTTYZXSU6KF3'
    	AWS_SECRET_ACCESS_KEY = sh 'export AWS_SECRET_ACCESS_KEY=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'
    	AWS_DEFAULT_REGION = sh 'export AWS_DEFAULT_REGION=ap-south-1'
    }

    stages {
        stage ('Build Stage') {
            steps {
            	echo "Setting current build to ${CURRENT_BUILD_DISPLAY}"
            	echo 'Project build started.'
    			sh './gradlew build'
  				echo 'Project build finished.'
            }
        }
        stage ('Auth Token Stage') {
            steps {
            	//withAWS(credentials: 'prashanttballal@gmail.com',  region: 'ap-south-1') {
            	//	sh 'aws configure'
            	//}
            	sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
				sh 'aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
				sh 'aws configure set default.region $AWS_DEFAULT_REGION'
           		sh 'export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 --query authorizationToken --output text`'
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