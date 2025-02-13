pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github_token',
                    url: 'https://github.com/KavyashreeTR/FlaskJenkinsApp.git'

            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Stop Existing Flask App') {
            steps {
                sh 'pkill -f "python app.py" || true'  // Stop if running
            }
        }

        stage('Run Flask App') {
            steps {
                sh 'nohup python app.py &'
            }
        }
    }
}
