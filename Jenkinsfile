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
    }
}