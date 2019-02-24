pipeline {
    agent any

    stages {
        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    "Check connectivity": {
                        sh "ansible centos -m ping"
                    },
                    "Check syntax": {
                        sh "ansible-playbook configure_nginx.yml --syntax-check"
                    }
                )
            }
        }
        stage('Run Nginx Role') {
            steps {
                ansiblePlaybook(
                    inventory: 'hosts',
                    playbook: 'configure_nginx.yml'
                )
            }
        }
    }
    post ('Check response from nginx') {
        success {
            sh "curl -I --connect-timeout 3 http://116.203.124.3:80"
        }
    }
}
