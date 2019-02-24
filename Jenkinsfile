pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = "${WORKSPACE}/configure_nginx.yml"
    }

    stages {
        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    "Check connectivity": {
                        sh "ansible centos -m ping"
                    },
                    "Check syntax": {
                        sh "ansible-playbook ${ANSIBLE_PLAYBOOK} --syntax-check"
                    }
                )
            }
        }
        stage('Run Nginx Role') {
            steps {
                sh "ansible-playbook configure_nginx.yml"
            }
        }
    }
    post ('Check response from host') {
        success {
            sh "curl -I --connect-timeout 3 http://116.203.124.3:80"
        }
    }
}
