pipeline {
    agent any

    stages {
        stage ('Build Stage') {
            steps {
            	echo 'Project build started.'
           		withGradle {
    				bat './gradlew build'
  				}
  				echo 'Project build finished.'
            }
        }
        stage ('Publish Stage') {
            steps {
            	echo 'Publishing dependencies to AWS CodeArtifact started.'
           		withGradle {
    				bat './gradlew publish'
  				}
  				echo 'Publishing dependencies to AWS CodeArtifact finished.'
            }
        }
    }
}