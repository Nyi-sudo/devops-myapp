pipeline {
    agent any
	
    tools {
	git 'Default'
    }

    environment {
        DOCKER_IMAGE = "nyinyi1166/myapp"
        VERSION = "${2}"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Nyi-sudo/devops-myapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$VERSION .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin
                    docker push $DOCKER_IMAGE:$VERSION
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl set image deployment/myapp myapp=$DOCKER_IMAGE:$VERSION
                '''
            }
        }
    }
}
