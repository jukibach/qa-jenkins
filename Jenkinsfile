pipeline {

    agent{
        docker {
            image 'alpinelinux/ansible'
        }
    }
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }
    stages {

        stage('Deploy Postgresql container') {
           
            steps {
                withCredentials([file(credentialsId: 'qa_ansible_key', variable: 'qa_ansible_key')]) {
                    sh 'ls -la'
                    sh "cp /$qa_ansible_key qa_ansible_key"
                    sh 'cat qa_ansible_key'
                    sh 'ansible --version'
                    sh 'ls -la'
                    sh 'chmod 400 qa_ansible_key '
                    sh 'ansible-galaxy collection install -p community.docker collections'
                    sh 'sleep 15'
                    sh 'ansible-playbook -i hosts --private-key qa_ansible_key playbook.yml'
            }
            }
        }
        
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}