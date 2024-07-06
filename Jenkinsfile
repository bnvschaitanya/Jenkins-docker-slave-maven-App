pipeline {
    agent none
    parameters {
        string(name: 'NAME', defaultValue: 'my life', description: 'Enter your name: ')
    }
    stages {
        stage('Checkout') {
            agent {
                label 'myslave1'
            }
            steps {
                echo 'Starting checkout stage'
                git url: '---git url---', branch: 'master'
                sh 'ls -l'
                sh 'pwd'
                stash includes: 'pom.xml, src/**', name: 'sourceCode'
            }
        }
        stage('Build and Package') {
            agent {
                label 'myslave2'
            }
            steps {
                echo 'Starting build and package stage'
                unstash 'sourceCode'
                sh 'mvn clean package'
                stash includes: 'target/*.jar', name: 'myJar'
            }
        }
        stage('Deploy Package') {
            agent {
                label 'myslave3'
            }
            steps {
                echo 'Starting deploy package stage'
                unstash 'myJar'
                // Commands to deploy the application
                sh 'scp target/*.jar user@server:/path/to/deploy'
                sh 'ssh user@server "cd /path/to/deploy && ./deploy.sh"'
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            // Clean up commands
            deleteDir()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
