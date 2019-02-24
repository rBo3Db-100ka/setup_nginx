pipeline {
    agent any

    environment {
        PLAYBOOK = "configure_nginx.yml"
        INVENTORY = "hosts"
    }


    stages {
        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    "Check connectivity": {
                        sh "ansible centos -m ping"
                    },
                    "Check syntax": {
                        ansiblePlaybook(
                            inventory: "${INVENTORY}",
                            playbook: "${PLAYBOOK}",
                            extras: '--syntax-check'
                        )
                    }
                )
            }
        }
        stage('Run Nginx Role') {
            steps {
                ansiblePlaybook(
                    inventory: "${INVENTORY}",
                    playbook: "${PLAYBOOK}",
                    extraVars: [verbose: True]
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
