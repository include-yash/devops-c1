pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\pip install --upgrade pip'
                bat 'venv\\Scripts\\pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                bat 'venv\\Scripts\\python -m unittest discover -s .'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat 'if not exist python-app-deploy mkdir python-app-deploy'
                bat 'copy /Y app.py python-app-deploy\\'
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application in background...'
                bat 'start /B venv\\Scripts\\python python-app-deploy\\app.py > python-app-deploy\\app.log 2>&1'
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing deployed application...'
                bat 'venv\\Scripts\\python test_app.py'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
