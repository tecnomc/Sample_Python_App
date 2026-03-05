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
                sh 'docker build -t python-flask-app .'
            }
        }
        stage('Deploy to App Server') {
            steps {
               sh '''ssh -i /var/lib/jenkins/.ssh/aws.pem -o StrictHostKeyChecking=no ec2-user@100.54.90.244 docker run -d -p 5000:5000 python-flask-app 
                   "docker stop python-flask-app || true
                    docker rm python-flask-app || true
                    docker run -d -p 5000:5000 --name python-flask-app python-flask-app"'''
             }
          }       
    }
}
