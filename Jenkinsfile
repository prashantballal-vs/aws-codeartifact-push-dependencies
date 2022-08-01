pipeline {
    agent any

    stages {
        stage ('Build Stage') {
            steps {
            	echo '1'
           		withGradle {
    				bat './gradlew build'
  				}
            }
        }
        stage ('Publish Stage') {
            steps {
            	echo '2'
           		withGradle {
           			//bat './D://PRACTICE//aws-codeartifact-push-dependencies'
           			bat './for /f %i in ("aws codeartifact get-authorization-token --domain company-domain --domain-owner 121322708209 --query authorizationToken --output text") do set CODEARTIFACT_AUTH_TOKEN=%i'
    				bat './gradlew publish'
  				}
            }
        }
    }
}