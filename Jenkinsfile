pipeline {
    agent any

    stages {
        stage('Check ansible connectivity and syntax') {
            steps {
                sh "ansible centos -m ping"
            }
        }
        stage('Run Nginx Role') {
            steps {
                sh "ansible-playbook configure_nginx.yml"
            }
        }
        stage('Check response from host') {
            steps {
                 def http_code = "url -s -I -w '%{http_code}' http://116.203.124.3:80"
                 sh "echo ${http_code}""
            }
        }
    }
}
