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
                        ansiblePlaybook(
                            inventory: 'hosts',
                            playbook: 'configure_nginx.yml',
                            extras: '--syntax-check'
                        )
                    }
                )
            }
        }
        stage('Run Nginx Role') {
            steps {
                ansiblePlaybook(
                    inventory: 'hosts',
                    playbook: 'configure_nginx.yml',
                    extraVars: [verbose: 1]
                )
            }
        }
    }
    post ('Check response from nginx') {
        success {
            sh "curl -s -I --connect-timeout 3 http://116.203.124.3:80"
        }
    }
}
