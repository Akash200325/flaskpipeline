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
                call %VENV%\Scripts\activate
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                bat '''
                call %VENV%\Scripts\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Stop Existing Flask App') {
            steps {
                echo "Stopping existing Flask app (if running)..."
                bat '''
                tasklist | findstr /I "python.exe" && taskkill /F /IM python.exe /T || echo No existing process found
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                echo "Starting Flask app..."
                bat '''
                call %VENV%\Scripts\activate
                python -m pip install --upgrade pip
                start /B python app.py > nohup.out 2>&1
                echo "Flask app started successfully!"
                '''
            }
        }
    }
}
