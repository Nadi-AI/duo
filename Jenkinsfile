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
                sh '''
                docker build -t scribral/duo-flask:latest .
                docker build -t scribral/duo-nginx:latest ./nginx
               
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push scribral/duo-flask:latest
                docker push scribral/duo-nginx:latest
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments --namespace=development
                '''
            }
        }
        stage('Rollout restart') {
            steps {
                sh '''
                kubectl rollout restart deployment --namespace=development flask-deployment
                '''
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

