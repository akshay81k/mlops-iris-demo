pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3.11 -m venv .jenkins-venv
                    . .jenkins-venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Retrain Model') {
            steps {
                sh '''
                    . .jenkins-venv/bin/activate
                    python retrain.py
                '''
            }
        }

        stage('Deploy Model') {
            steps {
                sh '''
                    sudo cp iris_model.pkl /home/ubuntu/app/iris_model.pkl
                '''
            }
        }
    }

    post {
        always {
            sh 'rm -rf .jenkins-venv'
        }
    }
}
