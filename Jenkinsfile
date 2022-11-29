pipeline {
    agent any
    stages {
        stage('Build') {
            steps { 
                sh '''
                docker build -f ./Dockerfile -t cmayman/flask-app:latest .
                '''
                // build the image from the Dockerfile
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push cmayman/flask-app:latest
                '''
                // push the image to the repository (based on the name image name in yaml)
            }
        }
        stage('Run') {
            steps {
                sh '''
                kubectl apply -f .
                '''
                //
            }
        }
        stage('Clean Up') {
            steps {
                sh '''
                docker system prune --force
                '''
                //remove any images not currently in use by a container
            }
        }
    }
}