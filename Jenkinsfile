#!/bin/env groovy
//@Library('jenkins-shared-library')
library identifier:'jenkins-shared-library@main',retriever:modernSCM(
[$class:'GitSCMSource',
remote:'https://github.com/python1951/jenkins-shared-library.git',
credentialsId:'github']
)
def gv

pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    buildImage 'qamarha28812/demo-app:jma-2.0'
                    dockerlogin()
                    dockerpush 'qamarha28812/demo-app:jma-2.0'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying the pipeline ok"
                    //gv.deployApp()
                }
            }
        }
    }   
}