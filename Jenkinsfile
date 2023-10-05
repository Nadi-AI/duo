pipeline {
    agent any
    stages {
        stage('Greeting and niceties') {
            steps {
                sh '''
                echo "Good Morning Nadia, let's get building!"
                '''
            }
        }        
        stage('Buildengg') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'develop') {
                sh '''
                docker build -t scribral/duo-flask:latest .
                docker build -t scribral/duo-flask:v$BUILD_NUMBER .
                docker build -t scribral/duo-nginx:latest ./nginx
                docker build -t scribral/duo-nginx:v$BUILD_NUMBER ./nginx
               
                '''                      
                    } else {
                        sh '''
                        echo "I don't need to do this"
                        '''

                    }


                }                

            }
        }
        stage('Push') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'develop') {
            
                sh '''
                docker push scribral/duo-flask:latest
                docker push scribral/duo-flask:v$BUILD_NUMBER
                docker push scribral/duo-nginx:latest
                docker push scribral/duo-nginx:v$BUILD_NUMBER
                '''
                    } else {
                        sh '''
                        echo "I don't need this negativity in my life"
                        '''

                      }
                }               
            }
        }
        stage('Deploy and rollout restart') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'develop') {
                            
                sh '''
                kubectl apply -f ./k8s-deployments --namespace=development
                kubectl rollout restart deployment --namespace=development flask-deployment
                '''
                    } else if (env.GIT_BRANCH == 'main') {
                        sh '''
                        kubectl apply -f ./k8s-deployments --namespace=development
                        kubectl rollout restart deployment --namespace=development flask-deployment
                        '''

                    }  else {
                        sh '''
                        echo "I don't need this negativity in my life"
                        '''

                      }              
                }                
            }
        }  
        stage('Clean Up') { 
            steps {
                sh '''
                docker system prune -a --force
                '''
            }
        }
    }
}

