pipeline {
    agent none
    parameters {
        string(name: 'x', defaultValue: 'my life', description: 'Enter your name: ')
    }
    stages {
        stage('Build') {
            agent {
                label 'myslavemaven'
            }
            steps {
                echo 'Starting build stage'
                git url: '---git url---', branch: 'master'
                sh 'ls -l'
                sh 'pwd'
                stash includes: 'pom.xml', name: 'myapp'
            }
        }
        stage('Test') {
            agent {
                label 'myslavemaven'
            }
            steps {
                echo 'Starting test stage'
                unstash 'myapp'
                sh 'mvn package'
                stash includes: 'target/*.jar', name: 'my.jar'
            }
        }
        stage('Deploy') {
            agent {
                label 'myslavemaven'
            }
            steps {
                echo 'Starting deploy stage'
                unstash
                sh 'java -jar target/*.jar'
            }
        }
    }
}
