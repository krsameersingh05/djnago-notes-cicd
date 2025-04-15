pipeline {
    agent any

    stages {
        stage('clone code') {
            steps {
                git branch: 'main', url: 'https://github.com/krsameersingh05/djnago-notes-cicd.git'
            }
        }
          stage('Build') {
            steps {
                sh 'docker build -t notes-app .'
            }
        }
          stage('Push to Docker hub') {
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
    sh """
        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
        docker tag notes-app $DOCKER_USERNAME/notes-app:latest
        docker push $DOCKER_USERNAME/notes-app:latest
    """
}

            }
        }
         stage('Deploy') {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
    }


