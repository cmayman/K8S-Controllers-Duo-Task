pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script { 
                    if (env.GIT_BRANCH == 'origin/development') {
                        sh '''
                        docker build -f ./Dockerfile -t maymanchris-flask-app:latest .
                        '''
                        // build the image from the Dockerfile
                    }
                    if (env.GIT_BRANCH == 'origin/main') {
                        echo 'no need to rebuild'
                        // Skip
                    }
                    else {
                        echo 'unknown branch'
                        // Skip
                    }          
                }
            }
        }
        stage('Push') {
             steps {
                script { 
                    if (env.GIT_BRANCH == 'origin/development') {
                        sh '''
                        docker push maymanchris-flask-app:latest
                        '''
                        // push the image to the repository
                    }
                    if (env.GIT_BRANCH == 'origin/main') {
                        echo 'nothing to push'
                        // Skip
                    }
                    else {
                        echo 'unknown branch'
                        // Skip
                    }          
                }
            }
        }
        stage('Run') {
             steps {
                script { 
                    if (env.GIT_BRANCH == 'origin/development') {
                        sh '''
                        kubectl apply -f . --namespace development
                        '''
                        // apply in development namespace
                    }
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl apply -f . --namespace production
                        '''
                        // apply in main namespace 
                    }
                    else {
                        echo 'unknown branch'
                        // Skip
                    }          
                }
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