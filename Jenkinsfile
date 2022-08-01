pipeline {
    agent any

    stages {
        stage ('Compile Stage') {
            steps {
            	echo '1'
           		sh 'gradle clean compile'
            }
        }
        
        stage ('Build Stage') {
            steps {
            	echo '1'
           		sh 'gradle build'
            }
        }
    }
}