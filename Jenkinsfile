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
    				bat './gradlew publish'
  				}
            }
        }
    }
}