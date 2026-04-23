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
                echo '📦 Installing Python dependencies...'
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running tests...'
                sh 'python3 -m pytest tests/ -v'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying application...'
                sh '''
                    # Kill old running instance if exists
                    pkill -f "python3 app.py" || true
                    
                    # Start fresh
                    nohup python3 app.py > app.log 2>&1 &
                    
                    # Give it 2 seconds to start
                    sleep 2
                    
                    # Confirm it's alive
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
