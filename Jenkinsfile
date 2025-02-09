pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/gradle-example"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'git@github.com:TechVishal-02/gradle-example.git'
            }
        }

        stage('Build with Gradle') {
            steps {
                sh './gradlew build'
            }
        }

        stage('Run Tests') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials', url: '']) {
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                '''
            }
        }
    }
}

