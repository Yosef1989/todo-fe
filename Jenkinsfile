pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f Docker-pipeline-front -t image-build --target build .'
            }
        }
        stage('Test stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f Docker-pipeline-front -t image-testing --target testing .'
            }
        }
        stage('Delivery stage') {
            steps {
                sh 'DOCKER_BUILDKIT=1 docker build -f Docker-pipeline-front -t yosihaimov/todo-fe:jenkins-$BUILD_NUMBER --target delivery .'
            }
        }
        stage('Cleanup') {
            steps {
                echo "Cleanup-stage"
                sh 'docker system prune -f'
            }
        }
        stage('push') {
            environment {
                docker_pass = credentials('dockerpass')
            }
            steps {
                    sh 'docker login -u yosihaimov -p ${docker_pass}'
                    sh "docker push yosihaimov/todo-be:jenkins-$BUILD_NUMBER"
        }
    }
}
}
