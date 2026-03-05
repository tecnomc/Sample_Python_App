pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/tecnomc/Sample_Python_App.git'
                
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t shilpa1819/python-flask-app:${BUILD_NUMBER} .'
            }
        }
        
        stage('push docker Image'){
            steps{
                withCredentials([usernamePassword(
                credentialsId: 'dockerhub'
                usernameVaraible: 'DOCKER_USER'
                passwordVaraible: 'DOCKER_PWD' )])
                 {
                sh '''
                   echo $DOCKER_PWD | docker login -u $DOCKER_USER --password-stdin
                   docker push shilpa1819/python-flask-app:${BUILD_NUMBER}
                   '''
           }
        }
        stage('Deploy to App Server') {
            steps {
               sh '''
               ssh -i /var/lib/jenkins/.ssh/aws.pem -o StrictHostKeyChecking=no ec2-user@100.54.90.244 " 
               docker stop python-flask-app || true &&
               docker rm python-flask-app || true &&
               docker pull shilpa1819/python-flask-app:${BUILD_NUMBER} &&
               docker run -d -p 5000:5000 --name python-flask-app shilpa1819/python-flask-app:${BUILD_NUMBER}
               "
               '''
           }
        }       
        stage('Clean old images'){
            steps {
               sh 'docker system prune -f' 

    }
}
