pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withGradle(gradle : 'gradle_7_4_1') {
                    sh 'gradle clean compile'
                }
            }
        }

        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'maven_3_5_0') {
                    sh 'mvn deploy'
                }
            }
        }
    }
}