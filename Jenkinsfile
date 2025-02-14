pipeline {
    agent any

    environment {
        VENV = 'venv'  // Virtual environment name
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git branch: 'main',
                    credentialsId: 'github_token',
                    url: 'https://github.com/kavyaramesh18/FlaskJenkinsApp.git'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                echo "Setting up virtual environment..."
                bat '''
                if exist %VENV% rmdir /s /q %VENV%
                python -m venv %VENV%
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                bat '''
                call %VENV%\\Scripts\\activate && python -m pip install --upgrade pip
                call %VENV%\\Scripts\\activate && pip install -r requirements.txt
                '''
            }
        }

        stage('Stop Existing Flask App') {
            steps {
                echo "Stopping existing Flask app (if running)..."
                bat '''
                for /f "tokens=2 delims=," %%A in ('tasklist /FI "IMAGENAME eq python.exe" /FO CSV /NH') do (
                    echo Killing process %%A
                    taskkill /F /PID %%A >nul 2>&1
                ) || echo No existing process found
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                echo "Starting Flask app..."
                bat '''
                call %VENV%\\Scripts\\activate
                start /B python app.py > flask.log 2>&1
                echo Flask app started successfully!
                '''
            }
        }
    }
}
