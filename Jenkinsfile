pipeline {
    agent any
    
    environment {
    	CURRENT_BUILD_DISPLAY="0.1.${BUILD_NUMBER}"
    	PROJECT_FOLDER="."
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
            	sh 'export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE'
    			sh 'export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY'
    			sh 'ULT_REGION=us-west-2'
           		sh 'export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 --region ap-south-1 --query authorizationToken --output text`'
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