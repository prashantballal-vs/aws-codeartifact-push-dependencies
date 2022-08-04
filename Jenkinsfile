pipeline {
    agent any
    
    environment {
    	CURRENT_BUILD_DISPLAY = "0.1.${BUILD_NUMBER}"
    	PROJECT_FOLDER = "."
    	AWS_ACCESS_KEY_ID = "AKIARYP3FTTYZXSU6KF3"
    	AWS_SECRET_ACCESS_KEY = "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"
    	AWS_DEFAULT_REGION = "ap-south-1"
    	CODEARTIFACT_AUTH_TOKEN = ""
    	ppb11 = "Ballal"
    }

    stages {    
    	stage ('Clean Stage') {
            steps {
    			sh './gradlew clean'
            }
        }
        stage ('Build Stage') {
            steps {
            	echo "Setting current build to ${CURRENT_BUILD_DISPLAY}"
            	echo 'Project build started.'
    			sh './gradlew build'
  				echo 'Project build finished.'
            }
        }
        stage ('Auth Stage') {
            steps {
            	echo "AWS_ACCESS_KEY_ID: ${AWS_ACCESS_KEY_ID}"
            	echo "AWS_SECRET_ACCESS_KEY: ${AWS_SECRET_ACCESS_KEY}"
            	echo "AWS_DEFAULT_REGION: ${AWS_DEFAULT_REGION}"
            	sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
				sh 'aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
				sh 'aws configure set default.region $AWS_DEFAULT_REGION'
           		sh 'CODEARTIFACT_AUTH_TOKEN= aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 --query authorizationToken --output text'
            	echo "CODEARTIFACT_AUTH_TOKEN: ${env.CODEARTIFACT_AUTH_TOKEN}"
            	sh 'export ppb11=prashant'
            	echo "ppb11: ${env.ppb11}"
            	echo sh(script: 'env|sort', returnStdout: true)
            	sh 'printenv ppb11'
            	sh 'printenv CODEARTIFACT_AUTH_TOKEN'
            }
        }
        stage ('Publish Stage') {
            steps {
            	echo "Publishing dependencies to AWS CodeArtifact started."
            	echo "CODEARTIFACT_AUTH_TOKEN: ${env.CODEARTIFACT_AUTH_TOKEN}"
    			sh './gradlew publish'
  				echo 'Publishing dependencies to AWS CodeArtifact finished.'
            }
        }
    }
}