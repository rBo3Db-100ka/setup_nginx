pipeline {
    agent any

    stages {
        stage('Check ansible connectivity and syntax') {
            parallel {
                steps {
                    sh "ansible centos -m ping"
                }
                steps {
                    sh "echo 1"
                }
             }
        }
        stage('Run Nginx Role') {
            steps {
                sh "ansible-playbook configure_nginx.yml"
            }
        }
        stage('Check response from host') {
            steps {
                  curl -s -I -w '%{http_code}' http://116.203.124.3:80
#                 def http_code = "curl -s -I -w '%{http_code}' http://116.203.124.3:80"
#                 sh "echo ${http_code}"
            }
        }
    }
}
