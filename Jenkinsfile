pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "Docker-username/flask-app"
        EC2_USER = "ec2-user"
        EC2_HOST = "your-EC2-IP"
        //add these credentials in jenkins and get ID from there to put in the jenkins file
        DOCKERHUB_CREDENTIALS_ID = "DOCKERHUB_CREDENTIALS_ID"
        EC2_SSH_CREDENTIALS_ID = "EC2_SSH_CREDENTIALS_ID"
        GITHUB_CREDENTIALS_ID = "EC2_SSH_CREDENTIALS_ID"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yassa27/project-docker-ec2-test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }
        stage('Test Docker Image') {
            steps {
                script {
                    docker.image("${env.DOCKER_IMAGE}:${env.BUILD_ID}").inside {
                        sh 'echo "Running tests..."'
                        sh 'pip install pytest'
                        sh 'pytest tests/'
                        sh '''
                        nohup flask run &
                        sleep 5
                        curl -f http://localhost:5000/ || exit 1
                        '''
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent([EC2_SSH_CREDENTIALS_ID]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                    docker pull ${DOCKER_IMAGE}:latest &&
                    docker stop flask-app || true &&
                    docker rm flask-app || true &&
                    docker run -d --name flask-app -p 5000:5000 ${DOCKER_IMAGE}:latest
                    '
                    '''
                }
            }
        }
    }
}
