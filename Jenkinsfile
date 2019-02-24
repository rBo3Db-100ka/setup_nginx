pipeline {
    agent any

    stages {
        stage('Check ansible connectivity and syntax') {
            steps {
                sh "pwd"
                sh "ls -la"
            }
        }
        stage('Run Nginx role') {
            steps {
                sh echo "ansible-playbook configure_nginx.yml"
            }
        }
        stage('Check response from host') {
            steps {
                script {
                     echo   "curl http://116.203.124.3:80"
                }
            }
        }
    }
}
