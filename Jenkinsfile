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
                docker build -t scribral/duo-flask:v$BUILD_NUMBER .
                docker build -t scribral/duo-nginx:latest ./nginx
                docker build -t scribral/duo-nginx:v$BUILD_NUMBER ./nginx
               
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push scribral/duo-flask:latest
                docker push scribral/duo-flask:v$BUILD_NUMBER
                docker push scribral/duo-nginx:latest
                docker push scribral/duo-nginx:v$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments --namespace=production
                '''
            }
        }
        stage('Rollout restart') {
            steps {
                sh '''
                kubectl rollout restart deployment --namespace=production flask-deployment
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

