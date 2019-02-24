pipeline {
    agent any

    stages {

        stage('Check ansible connectivity and syntax') {
            steps {
                parallel (
                    check_connectivity: {
                        sh "ansible centos -m ping"
                    },
                    check_syntax: {
                        sh "echo Done"
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
            sh "curl -s -o /dev/null -I -w '%{http_code}' http://116.203.124.3:80"
        }
    }
}
