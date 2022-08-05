pipeline {
    agent any
    environment {
    	BUILD_VERSION = "1.${BUILD_NUMBER}"
    	GROUP_ID = "org.gradle.sample.jenkinsfile"
    	PROJECT_FOLDER = "."
    	AWS_ACCESS_KEY_ID = "AKIARYP3FTTYZXSU6KF3"
    	AWS_SECRET_ACCESS_KEY = "DKUJiymRe92WSW1SJ4qHDUnd6BAy0OPlF6FKN02j"
    	AWS_DEFAULT_REGION = "ap-south-1"
    }
    stages {    
        stage ('Clean & Build Stage') {
            steps {
            	echo "Started cleaning & building the project."
            	echo "Setting current build to ${BUILD_VERSION}"
    			sh './gradlew clean build'
  				echo "Finished cleaning & building the project."
            }
        }
        stage ('AWS Configuration Stage') {
            steps {
            	echo "Started configuring AWS."
            	sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID'
				sh 'aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY'
				sh 'aws configure set default.region $AWS_DEFAULT_REGION'
				echo "Finished configuring AWS."
            }
        }
        stage ('AWS CodeArtifact Token Stage') {
        	echo "Started creating authentication token for AWS CodeArtifact."
        	script {
				env.CODEARTIFACT_AUTH_TOKEN = "${sh(script:'aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 --query authorizationToken --output text', returnStdout: true).trim()}"
			}
			echo "Finished creating authentication token for AWS CodeArtifact."
        }
        stage ('Publish Dependencies Stage') {
            steps {
            	echo "Started publishing dependencies to AWS CodeArtifact."
    			sh './gradlew publish'
  				echo "Finished publishing dependencies to AWS CodeArtifact."
            }
        }
    }
}