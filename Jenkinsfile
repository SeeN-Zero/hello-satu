pipeline {
    agent any

    environment {
        MAVEN_REPO_VOLUME = 'maven-repo'
    }

    stages {
        stage('Create Maven Repo Volume') {
            steps {
                script {
                    sh 'docker volume create --name ${env.MAVEN_REPO_VOLUME}'
                }
            }
        }

        stage('Build JAR with Docker') {
            steps {
                script {
                    docker.image('maven:3.9.9-eclipse-temurin-21-alpine').inside("-u root -v ${env.MAVEN_REPO_VOLUME}:/root/.m2") {
                        sh 'mvn clean package -DskipTests'
                    }
                }
                archiveArtifacts artifacts: 'target/quarkus-app/quarkus-run.jar', allowEmptyArchive: true
            }
        }

        stage('Run JAR with Docker') {
            steps {
                script {
                    sh 'docker run --rm -v $(pwd)/target/quarkus-app:/app -w /app -p 3000:3000 eclipse-temurin:21 java -jar quarkus-run.jar'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
