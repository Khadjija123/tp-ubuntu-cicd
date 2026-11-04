pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Récupération du code') {
            steps {
                checkout scm
                sh 'ls -R'
            }
        }

        stage('Vérification de la syntaxe Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/inventory.ini ansible/deploy.yml --syntax-check'
            }
        }

        stage('Déploiement avec Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/inventory.ini ansible/deploy.yml'
            }
        }

        stage('Test du site') {
            steps {
                sh 'curl -f http://192.168.10.101'
                sh 'curl -f http://192.168.10.102'
            }
        }
    }

    post {
        success {
            echo 'Déploiement réussi sur slave1 et slave2'
        }
        failure {
            echo 'Échec du pipeline : consulter les logs'
        }
    }
}
