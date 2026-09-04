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
                sh printenv
            }
        }

        stage('Docker Build') {
            steps {
                withAnt(installation: 'ant-1.10.17') {
                    sh 'ant -f build.xml clean compile jar'
                }
            }
        }

        stage('Run') {
            steps {
                withAnt(installation: 'ant-1.10.17') {
                    sh 'ant -f build.xml run'
                }
            }
        }
    }
}