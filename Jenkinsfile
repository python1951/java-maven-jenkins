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
        stage("incrementing version ") {
                    steps {
                        script {
                            sh "mvn build-helper:parse=version versions:set \
                                -D newVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit"
                            def match = readFile("pom.xml") =~ <version>(.+)</version>
                            def version = match[0][1]
                            env.IMAGE_NAME = "$version-$BUILD_NUMBER"

                        }
                    }
                }
        stage("build jar ..") {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    buildImage "qamarha28812/demo-app:jma-${IMAGE_NAME}"
                    dockerlogin()
                    dockerpush "qamarha28812/demo-app:jma-${IMAGE_NAME}"
                }
            }
        }
        stage("commit version update"){
            steps{
                script{
                withCredentials(usernamePassword(credentialsId:'github',usernameVariable:'USER',passwordVariable:'PASS')
                    sh 'git config --global user.email "jenkins@example.com"'
                     sh 'git config --global user.name "jenkins"'


                    sh "git status"
                    sh "git branch"
                    sh "git config --list"
                    sh "git remote set-url origin https://${USER}:${PASS}@github.com/python1951/java-maven-jenkins.git"
                    sh 'git add .'
                    sh 'git commit -m "jenkins commit"'
                    sh 'git push origin HEAD:test'


                }
        }}
        stage("deploy") {
            steps {
                script {
                    echo "deploying the pipeline..."
                    //gv.deployApp()
                }
            }
        }
    }   
}