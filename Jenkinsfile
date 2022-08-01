pipeline {

    agent any

    environment {
        GIT_BRANCH_LOCAL = sh (
                    script: "echo $GIT_BRANCH | sed -e 's|origin/||g'",
                    returnStdout: true
                ).trim()
        GIT_COMMIT_HASH = GIT_COMMIT.take(6)
        CURRENT_BUILD_DISPLAY="0.1.${BUILD_NUMBER}"
        PROJECT_FOLDER="."
        PROJECT_NAME="aws-codeartifact-pull-dependencies"

		//Adding default values for env variables
		GRADLE_JAVA_HOME="/opt/java/openjdk"
		//GCP_SA="vsi-ops-gcr-service-account@mgcp-1308657-vsi-operations.iam.gserviceaccount.com"
		//GCP_PROJECT="mgcp-1308657-vsi-operations"
        SONAR_JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
        SONAR_HOST="http://sonar-sonarqube:9000/sonar"

    }
    stages {
        stage('Clean Workspace') {
            steps {
                echo "Setting current build to ${CURRENT_BUILD_DISPLAY}"
                script {
                    currentBuild.displayName = "${CURRENT_BUILD_DISPLAY}"

                    currentBuild.description = """Branch - ${GIT_BRANCH_LOCAL}
                        Commit# - ${GIT_COMMIT_HASH}"""
                }
                dir("${PROJECT_FOLDER}") {
                    echo "Changed directory to ${PROJECT_FOLDER}"
                    echo 'Cleaning up Work Dir...'
                    sh './gradlew clean -PbuildName=${CURRENT_BUILD_DISPLAY} -Dorg.gradle.java.home=${GRADLE_JAVA_HOME}'
                    sh 'mkdir -p build/libs && touch build/libs/${PROJECT_NAME}-${CURRENT_BUILD_DISPLAY}.jar'
                }
            }
        }

        stage('Tests And Code Quality') {
            steps {
                dir("${PROJECT_FOLDER}") {
                    echo 'Running Tests and SonarQube Analysis'
                     withCredentials([string(credentialsId: 'sonar_key', variable: 'SONAR_KEY')]) {
                        sh '''
                          ./gradlew -i sonarqube -Dorg.gradle.java.home=${SONAR_JAVA_HOME}  \
                                                 -Dsonar.host.url=${SONAR_HOST}  \
                                                 -PbuildName=${CURRENT_BUILD_DISPLAY} \
                                                 -Dsonar.login=$SONAR_KEY \
                                                 -DprojectVersion=${CURRENT_BUILD_DISPLAY}
                        '''
                     }
                    echo 'Ran SonarQube Analysis successfully'
                }
            }
        }

        stage('ContainerRegistry') {
            steps {
                withCredentials([file(credentialsId: 'vsi-ops-gcr', variable: 'SECRET_JSON')]) {
                    echo 'Activating gcloud SDK Service Account...'
                    sh 'gcloud auth activate-service-account $GCP_SA --key-file $SECRET_JSON --project=$GCP_PROJECT'
                    sh 'gcloud auth configure-docker'
                    echo 'Activated gcloud SDK Service Account'
                    dir("${PROJECT_FOLDER}") {
                        echo "Pushing image to GCR with tag ${CURRENT_BUILD_DISPLAY}..."
                        sh "./gradlew jib -PbuildName=${CURRENT_BUILD_DISPLAY} -PgcpProject=${GCP_PROJECT} -Dorg.gradle.java.home=${GRADLE_JAVA_HOME}"
                        echo "Pushed image to GCR with tag ${CURRENT_BUILD_DISPLAY} successfully"
                    }
                    echo 'Revoking gcloud SDK Service Account...'
                    sh "gcloud auth revoke ${GCP_SA}"
                    echo 'Revoked gcloud SDK Service Account'
                }
            }
        }

    }
    post {
        success {
            dir("${PROJECT_FOLDER}") {
                echo 'Archiving build artifacts...'
                archiveArtifacts artifacts: "build/libs/*.jar, config/**/*", fingerprint: true, onlyIfSuccessful: true
                echo 'Archived build artifacts successfully'
                echo 'Publising Jacoco Reports...'
                jacoco( 
                    execPattern: 'build/jacoco/*.exec',
                    classPattern: 'build/classes',
                    sourcePattern: 'src/main/java',
                    exclusionPattern: 'src/test*'
                )
                echo 'Published Jacoco Reports successfully'
            }
            echo 'Cleaning up workspace...'
            deleteDir()
        }
        failure {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
        aborted {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
    }
}