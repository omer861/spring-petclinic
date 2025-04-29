pipeline {
    agent any

    environment {
        MYSQL_URL = 'jdbc:mysql://database-1.cz466cygikg5.ap-south-1.rds.amazonaws.com:3306/petclinic'
        MYSQL_HOST = 'database-1.cz466cygikg5.ap-south-1.rds.amazonaws.com'
        MYSQL_PORT = '3306'
        MYSQL_USER = 'petclinic'
        MYSQL_PASS = credentials('MYSQL_PASS')  // Jenkins credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Checkout the source code from repository
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build with Gradle
                    sh './gradlew clean build'
                }
            }
        }

        stage('Store Artifact') {
            steps {
                script {
                    // Archive the build artifact (e.g., JAR file)
                    archiveArtifacts artifacts: 'build/libs/*.jar', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
    }
}
