
pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                bat 'python -m py_compile app.py test_app.py'
            }
        }

        stage('Test') {
            steps {
                bat 'python -m unittest -v test_app'
            }
        }

        stage('Package') {
            steps {
                bat 'if not exist dist mkdir dist'
                bat 'tar -a -c -f dist\\python-app.zip app.py test_app.py requirements.txt'
                archiveArtifacts artifacts: 'dist/python-app.zip'
            }
        }
    }

    post {
        success {
            echo 'Python CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
