pipeline {
    agent any
    stages {
        stage('Pytest') {
            agent { 
                label 'tester'
            }            
            steps {
                //
                git branch: 'main', url: 'https://github.com/Jamalh8/Travel-app.git'
                sh '''#!/bin/bash
                python3 -m venv venv
                source venv/bin/activate
                pip3 install -r requirements.txt
                pip3 install pytest pytest-cov
                python3 -m pytest tests/test_app.py --cov=application'''
            }
        }
        stage('Pip install') {
            agent { 
                label 'runner'
            }
            steps {
                //
                git branch: 'main', url: 'https://github.com/Jamalh8/Travel-app.git'
                sh '''#!/bin/bash
                python3 -m venv venv
                source venv/bin/activate
                pip3 install -r requirements.txt
                echo "installed requirements"'''
            }
        }
        stage('Deploy') {
            agent { 
                label 'runner'
            }            
            steps {
                //
                git branch: 'main', url: 'https://github.com/Jamalh8/Travel-app.git'
                sh '''#!/bin/bash
                if [ -f  /tmp/gunicornpidfilepidfile ]
                  then kill $(cat /tmp/gunicornpidfilepidfile)
                fi
                source venv/bin/activate
                echo "exporting environments"
                export DATABASE_URI='sqlite:///testdb'
                export MY_KEY=12345
                JENKINS_NODE_COOKIE=nokill python3 -m gunicorn -D -b  0.0.0.0:5000 -w 4 application:app  -p gunicornpidfile --log-file FILE'''
            }
        }
    }
}