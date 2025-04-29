pipeline {
    agent any

    tools {
        jdk '17'  // Match Java 17 from build.gradle
    }

    environment {
        MYSQL_URL = 'jdbc:mysql://database-1.cz466cygikg5.ap-south-1.rds.amazonaws.com:3306/petclinic'
        MYSQL_HOST = 'database-1.cz466cygikg5.ap-south-1.rds.amazonaws.com'
        MYSQL_PORT = '3306'
        MYSQL_USER = 'petclinic'
        MYSQL_PASS = credentials('MYSQL_PASS')
        SPRING_PROFILES_ACTIVE = 'mysql'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test DB Connection') {
            steps {
                script {
                    sh "mysql -h ${MYSQL_HOST} -P ${MYSQL_PORT} -u ${MYSQL_USER} -p${MYSQL_PASS} -e 'SELECT 1;'"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Pass environment variables to Gradle and Spring Boot
                    sh '''
                        ./gradlew clean build \
                        -Dspring.profiles.active=${SPRING_PROFILES_ACTIVE} \
                        -Dspring.datasource.url=${MYSQL_URL} \
                        -Dspring.datasource.username=${MYSQL_USER} \
                        -Dspring.datasource.password=${MYSQL_PASS}
                    '''
                }
            }
            post {
                always {
                    junit 'build/test-results/test/*.xml'  // Archive test reports
                }
            }
        }

        stage('Store Artifact') {
            steps {
                script {
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
