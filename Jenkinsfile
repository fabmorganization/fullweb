pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/fabmorganization/fullweb.git'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository with Submodules') {
            steps {
                sh "git clone --recurse-submodules ${env.GIT_REPO}"
                dir('fullweb') {
                    sh 'git submodule update --init --recursive'
                }
            }
        }

        stage('Stop Running Containers') {
            steps {
                dir('fullweb') {
                    sh 'docker-compose down'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                dir('fullweb') {
                    sh 'docker-compose build'
                }
            }
        }

        stage('Build App') {
            steps {
                dir('fullweb') {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Application successfully deployed to production.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more information.'
        }
    }
}