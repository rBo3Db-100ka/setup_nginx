pipeline {
    agent any

    stages {

        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    a: {
                        sh "ansible centos -m ping"
                    },
                    b: {
                        sh "echo 1"
                    }
                )
            }
        }

        stage('Run Nginx Role') {
            steps {
                sh "ansible-playbook configure_nginx.yml"
            }
        }

        stage('Check response from host') {
            steps {
                  sh "curl -s -I -w '%{http_code}' http://116.203.124.3:80"
            }
        }
    }
}
