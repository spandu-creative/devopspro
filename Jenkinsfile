pipeline {
    agent any

    environment {
        REPO = 'https://github.com/Become-DevOps/proj.git'
        IMAGE = 'html-app'
        PORT = '8080'
    }

    stages {
        stage('Clone Project') {
            steps {
                git branch: 'main', url: "${REPO}"
            }
        }

        stage('Install Docker on QA Node') {
            steps {
                sh 'ansible webservers -m shell -a "which docker || curl -fsSL https://get.docker.com | sh" --become'
            }
        }

        stage('Build Docker Image on QA') {
            steps {
                sh 'ansible webservers -m shell -a "cd /home/sree/devopspro && docker build -t ${IMAGE} ." --become'
            }
        }

        stage('Run Docker Container on QA') {
            steps {
                sh 'ansible webservers -m shell -a "docker rm -f ${IMAGE} || true" --become'
                sh 'ansible webservers -m shell -a "docker run -d -p ${PORT}:80 --name ${IMAGE} ${IMAGE}" --become'
            }
        }
    }
}
