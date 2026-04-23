pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Pulling latest code from GitHub...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📦 Setting up virtual environment and installing dependencies...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running tests...'
                sh '''
                    . venv/bin/activate
                    python3 -m pytest tests/ -v
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying application...'
                sh '''
                    pkill -f "python3 app.py" || true
                    sleep 1
                    . venv/bin/activate
                    nohup python3 app.py > app.log 2>&1 &
                    sleep 2
                    curl -s http://localhost:5000/health
                '''
            }
        }

    }

    post {
        success {
            echo '✅ Pipeline passed — app is live!'
        }
        failure {
            echo '❌ Pipeline failed — check the stage logs above'
        }
    }
}
