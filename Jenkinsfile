pipeline {
    agent none
    parameters {
        string(name: 'x', defaultValue: 'my life', description: 'Enter your name: ')
    }
    stages {
        stage('Build') {
            agent {
                label 'myslave1'
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
                label 'myslave2'
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
                label 'myslave3'
            }
            steps {
                echo 'Starting to deploy stage'
                unstash
                sh 'java -jar target/*.jar'
                // Commands to deploy the application
                sh 'scp target/myapp.jar user@server:/path/to/deploy'
                sh 'ssh user@server "cd /path/to/deploy && ./deploy.sh"'
            }
        }
    }
}
