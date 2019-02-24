pipeline {
    agent any

    stages {
        stage('Check ansible connectivity and syntax') {
            "parallels"
            steps {
                check sytnax and connect
            }
        }
        stage('Run Nginx role') {
            steps {
                sh "ansible-playbook configure_nginx.yml"
            }
        }
        stage('Check response from host') {
            steps {
                script {
                     curl http://116.203.124.3:80
                }
            }
        }
    }
}
