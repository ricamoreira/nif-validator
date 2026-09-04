#!groovy
/* nif-validator project
 * 2026-09-04 RIcardo Moreira
 * CI/CD on project
 */

pipeline {
    agent { label 'linux' }

    environment {
        HOME = "${env.WORKSPACE}"
    }

    stages {
        stage('Setup') {
            steps {
                sh 'printenv'
            }
        }

        stage('Create docker environment') {
            agent {
                docker {
                    image 'python:3.11-slim'
                    reuseNode true
                }
            }
            steps {
                sh """
                pip install --user -r requirements.txt
                pip install --user -r requirements-test.txt
                """
            }
        }

        stage('Unit tests') {
            agent {
                docker {
                    image 'python:3.11-slim'
                    reuseNode true
                }
            }
            steps {
                sh 'python3 -m pytest --junitxml results.xml tests/'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'results.xml', fingerprint: true
                    junit 'results.xml'
                }
            }
        }
    }
}