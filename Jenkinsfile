pipeline {
    agent any
    
    parameters {
        string(name: "Test", description: "Customer")
    }

    environment {
        CONFIGURE_NGINX = "configure_nginx.yml"
        CHECK_CONNECTIVITY = "check_connectivity.yml"
        INVENTORY = "hosts"
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
