pipeline {
    agent any

    environment {
        PLAYBOOK = "configure_nginx.yml"
        INVENTORY = "hosts"
    }

    stages {
        stage("CHECK ANSIBLE CONNECTIVITY AND SYNTAX") {
            steps {
                parallel (
                    "CHECK CONNECTIVITY": {
                        sh "ansible centos -m ping"
                    },
                    "CHECK PLAYBOOK SYNTAX": {
                        ansiblePlaybook(
                            inventory: "${INVENTORY}",
                            playbook: "${PLAYBOOK}",
                            extras: '--syntax-check'
                        )
                    }
                )
            }
        }
        stage("RUN NGINX ROLE") {
            steps {
                ansiblePlaybook(
                    inventory: "${INVENTORY}",
                    playbook: "${PLAYBOOK}",
                    extraVars: [verbose: 1]
                )
            }
        }
    }
    post ("CHECK RESPONSE FROM NGINX") {
        success {
            sh "curl -s -I --connect-timeout 3 http://116.203.124.3:80"
        }
    }
}
