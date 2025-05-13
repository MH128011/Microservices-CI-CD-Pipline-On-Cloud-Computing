pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Clone GITHUB Repository') {
            steps {
                git credentialsId: 'your-credentials-id', url: 'https://github.com/dig24-bit/ycesproject.git'
            }
        }

        stage('Copy source code to Docker swarm') {
            steps {
                sh 'ansible-playbook -i inventory.ini playbook-to-copy-data-to-docker.yml --user=jenkins'
            }
        }

        stage('Build & Push the new Image to Dockerhub') {
            steps {
                sh 'ansible-playbook -i inventory.ini playbook-to-push.yml --user=jenkins'
            }
        }

        stage('Deploying new Service in Docker Swarm') {
            steps {
                sh 'ansible-playbook -i inventory.ini playbook-to-deploy.yml --user=jenkins'
            }
        }
    }
}
