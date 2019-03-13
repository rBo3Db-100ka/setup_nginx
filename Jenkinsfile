pipeline {
    agent any

    parameters {
        string(
            name: "CONFIGURE_NGINX",
            description: "nginx playbook"
        )_
        string(
            name: "CHECK_CONNECTIVITY",
            description: "check conn"
        )
        string(
            name: "inventory",
            description: "inventory file"
        )
    }

    stages {
        stage("CHECK ANSIBLE CONNECTIVITY AND SYNTAX") {
            steps {
                parallel (
                    "CHECK CONNECTIVITY": {
                        ansiblePlaybook(
                            inventory: "${INVENTORY}",
                            playbook: "${CHECK_CONNECTIVITY}"
                        )
                    },
                    "CHECK PLAYBOOK SYNTAX": {
                        ansiblePlaybook(
                            inventory: "${INVENTORY}",
                            playbook: "${CONFIGURE_NGINX}",
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
                    playbook: "${CONFIGURE_NGINX}",
                    extraVars: [customer: altamedica]
                )
            }
        }
    }

    post ("CHECK RESPONSE FROM NGINX") {
        success {
            sh "some check to get zabbix agent/proxy status"
        }
    }
}
