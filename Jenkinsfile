pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK = sh(returnStdout: true, script: "which ansible-playbook")
        PLAYBOOK = "${WORKSPACE}/configure_nginx.yml"
    }

    stages {
        stage('output_version') {
            steps {
                echo "${ANSIBLE_PLAYBOOK} ${PLAYBOOK} --syntax-check"
            }
        }
        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    "Check connectivity": {
                        sh "ansible centos -m ping"
                    },
                    "Check syntax": {
                        sh "${ANSIBLE_PLAYBOOK} ${PLAYBOOK} --syntax-check"
                    }
                )
            }
        }
        stage('Run Nginx Role') {
            steps {
                sh "${ANSIBLE_PLAYBOOK} ${PLAYBOOK}"
            }
        }
    }
    post ('Check response from host') {
        success {
            sh "curl -I --connect-timeout 3 http://116.203.124.3:80"
        }
    }
}
