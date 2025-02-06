pipeline {
    agent any

    environment {
        VENV_PATH = "${WORKSPACE}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SyamalaKadmi/FlaskTest.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'pytest --junitxml=test-results.xml'
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
		expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    sh '''
                    nohup python app.py > flask.log 2>&1 &
                    '''
                }
            }
        }
    }

    post {
        success {
            emailext subject: "Jenkins Build Success: ${JOB_NAME}",
                     body: "Build for ${JOB_NAME} succeeded.",
                     to: 'syamala.kadimi@gmail.com'
        }
        failure {
            emailext subject: "Jenkins Build Failed: ${JOB_NAME}",
                     body: "Build for ${JOB_NAME} failed.",
                     to: 'syamala.kadimi@gmail.com'
        }
    }
}
