@Library("shared") _
pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'This is clonning code'
                git url : "https://github.com/naveed0057/django-notes-app.git", branch:'main'
                echo 'code clone successfully'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the image'
                sh 'whoami'
                sh 'docker build -t notes-app:latest .'
            }
        }

        stage('push to DockerHub') {
            steps {
                echo 'This is pushing the image to Docker Hub'
                withCredentials([usernamePassword(
                    credentialsId:'dockerHubcred',
                    passwordVariable:'DOCKER_HUB_PASS',usernameVariable:'DOCKER_HUB_USER')]){
                sh 'docker login -u $DOCKER_HUB_USER -p $DOCKER_HUB_PASS'
                sh 'docker tag notes-app:latest $DOCKER_HUB_USER/notes-app:latest'
                sh 'docker push $DOCKER_HUB_USER/notes-app:latest'
             }
           }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application'
                sh 'docker compose down && docker compose up -d'
            }
        }
    }
}
