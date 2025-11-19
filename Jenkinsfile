pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Choloooo/ci-cd-test.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    venv/bin/pip install --upgrade pip
                    venv/bin/pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh 'venv/bin/python -m pytest --ds=mysite.settings'
            }
        }
        stage('Deploy Locally') {
            steps {
                sh '''
                    # Apply migrations
                    venv/bin/python manage.py migrate
                    
                    # Collect static files (if needed)
                    venv/bin/python manage.py collectstatic --noinput
                    
                    # Start Django server in the background
                    nohup venv/bin/python manage.py runserver 0.0.0.0:8000 > django.log 2>&1 &
                '''
            }
        }
    }
}
