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

        stage('Coverage report') {
            agent {
                docker {
                    image 'python:3.11-slim'
                    reuseNode true
                }
            }
            steps {
                sh """
                python3 -m coverage run --source=. --omit=tests/* -m pytest tests
                python3 -m coverage report -m
                python -m coverage html
                """
            }
            post {
                always {
                    publishHTML(target:[
                        reportDir: 'htmlcov',
                        reportFiles: 'index.html',
                        reportName: 'Coverage report'
                    ])
                }
            }
        }

        stage('Deliver') {
            agent {
                docker {
                    image 'python:3.11-slim'
                    reuseNode true
                }
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHub',
                    usernameVariable: 'username',
                    passwordVariable: 'password'
                )]) {
                    sh """
                    docker login -u ${username} -p ${password}
                    docker build -t ${username}/nif-validator .
                    docker push ${username}/nif-validator
                    """
                }
            }
        }

        /*stage('Deploy') {
            steps {
                sh """
                ssh 63.176.151.129
                docker pull ricamoreira005/nif-validator
                docker run -d --name nif-validator -p 80:9046 cfreire70/nif-validator
                """
            }
        }*/
    }
}
