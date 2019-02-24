pipeline {
    agent any

    environment {
        PLAYBOOK = "${WORKSPACE}/configure_nginx.yml"
        GROUP = "centos"
    }

    stages {
        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    "Check connectivity": {
                        sh "ansible ${GROUP} -m ping"
                    },
                    "Check syntax": {
                        sh "ansible-playbook ${PLAYBOOK} --syntax-check"
                    }
                )
            }
        }
        stage('Run Nginx Role') {
            steps {
                sh "ansible-playbook ${PLAYBOOK}"
            }
        }
    }
    post ('Check response from host') {
        success {
            sh "curl -I --connect-timeout 3 http://116.203.124.3:80"
        }
    }
}
