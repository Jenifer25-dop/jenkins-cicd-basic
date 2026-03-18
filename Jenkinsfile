pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Jenifer25-dop/jenkins-cicd-basic.git'
            }
        }

        stage('Run Application') {
            steps {
                sh 'python3 app.py'
            }
        }
    }
}
