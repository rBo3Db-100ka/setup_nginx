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
        stage('Input Params') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }

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
