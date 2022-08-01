pipeline {
    agent any

    stages {
        stage ('Compile Stage') {
            steps {
           		sh 'gradle clean compile'
            }
        }
        
        stage ('Build Stage') {
            steps {
           		sh 'gradle build'
            }
        }
    }
}