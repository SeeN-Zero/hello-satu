pipeline {
    agent any
    stages {
        stage('Build JAR with Docker') {
            steps {
                script {
                    docker.image('maven:3.9.9-eclipse-temurin-21-alpine').inside("-v /path/to/your/local/repo:/root/.m2/repository") {
                        sh 'mvn clean package -DskipTests'
                    }
                }
                archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.image('maven:3.9.9-eclipse-temurin-21-alpine').inside {
                        sh 'docker build -f src/main/docker/Dockerfile.jvm'
                    }
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
