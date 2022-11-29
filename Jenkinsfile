pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script { 
                    if (env.BRANCH_NAME == 'development') {
                        sh '''
                        docker build -f ./Dockerfile -t cmayman/flask-app:latest .
                        '''
                        // build the image from the Dockerfile
                    }
                    if (env.BRANCH_NAME == 'main') {
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
                    if (env.BRANCH_NAME == 'development') {
                        sh '''
                        docker push cmayman/flask-app:latest
                        '''
                        // push the image to the repository
                    }
                    if (env.BRANCH_NAME == 'main') {
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
                    if (env.BRANCH_NAME == 'development') {
                        sh '''
                        kubectl apply -f . --namespace development
                        '''
                        // apply in development namespace
                    }
                    if (env.BRANCH_NAME == 'main') {
                        sh '''
                        kubectl apply -f . --namespace main
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