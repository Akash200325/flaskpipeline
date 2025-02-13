pipeline {
    agent any

    environment {
        VENV = 'venv'  // Virtual environment name
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github_token',
                    url: 'https://github.com/kavyaramesh18/FlaskJenkinsApp.git'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                bat '''
                python -m venv %VENV%
                call %VENV%\\Scripts\\activate
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                call %VENV%\\Scripts\\activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Stop Existing Flask App') {
            steps {
                bat '''
                taskkill /F /IM python.exe /T || echo No existing process found
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                bat '''
                call %VENV%\\Scripts\\activate
                start /B python app.py > nohup.out 2>&1
                '''
            }
        }
    }
}
