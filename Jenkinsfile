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
    }
}