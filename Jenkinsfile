pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                // Create virtual environment
                bat 'python -m venv venv'
                // Upgrade pip using Python to avoid Windows issues
                bat 'venv\\Scripts\\python -m pip install --upgrade pip'
                // Install dependencies from requirements.txt
                bat 'venv\\Scripts\\python -m pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                // Run all unit tests in the project folder
                bat 'venv\\Scripts\\python -m unittest discover -s .'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Create deploy folder if it doesn't exist
                bat 'if not exist python-app-deploy mkdir python-app-deploy'
                // Copy app.py to deploy folder
                bat 'copy /Y app.py python-app-deploy\\'
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application in background...'
                // Run Flask app in background and redirect logs
                bat 'start /B venv\\Scripts\\python python-app-deploy\\app.py > python-app-deploy\\app.log 2>&1'
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing deployed application...'
                // Run test script against deployed app
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
