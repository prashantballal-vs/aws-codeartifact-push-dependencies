pipeline {
    agent any

    stages {
        stage ('Compile Stage') {
            steps {
           		sh 'gradle clean compile'
            }
        }
        
        stage ('Compile Stage') {
            steps {
           		sh 'gradle build'
            }
        }

        /*stage ('Deployment Stage') {
            steps {
            	sh 'mvn deploy'
            }
        }*/
    }
}