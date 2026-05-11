pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')  // vérifie toutes les minutes
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ZayOnTop-1/devops-158-atomic-tp.git'
            }
        }

        stage('Pull latest code') {
            steps {
                dir('/home/pi-atomic/devops-158-atomic-tp') {
                    git branch: 'main', url: 'https://github.com/ZayOnTop-1/devops-158-atomic-tp.git'
                }
            }
        }

        stage('Install dependencies') {
            steps {
                dir('/home/pi-atomic/devops-158-atomic-tp') {
                    sh '''
                        source venv/bin/activate
                        pip install flask
                    '''
                }
            }
        }

        stage('Restart Flask app') {
            steps {
                script {
                    sh 'pkill -f "python app.py" || true'
                    sh '''
                        cd /home/pi-atomic/devops-158-atomic-tp
                        source venv/bin/activate
                        nohup python app.py > flask.log 2>&1 &
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Déploiement automatique réussi ! BRAVO DAMN'
        }
        failure {
            echo 'Échec du pipeline. - AIE AIE AIE CA PUE'
        }
    }
}
