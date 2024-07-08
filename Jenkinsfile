pipeline {
    agent none
    parameters {
        string(name: 'NAME', defaultValue: 'bnvschaitanya', description: 'Enter your name: ')
    }
    stages {
        stage('Checkout') {
            agent {
                label 'myslave1'
            }
            steps {
                echo "Starting checkout stage for ${params.NAME}"
                git url: '---git url---', branch: 'master'pipeline {
    agent none
    parameters {
        string(name: 'NAME', defaultValue: 'bnvschaitanya', description: 'Enter your name: ')
    }
    stages {
        stage('Checkout') {
            agent {
                label 'myslave1'
            }
            steps {
                echo "Starting checkout stage for ${params.NAME}"
                git url: '---git url---', branch: 'master'
                sh 'ls -l'
                sh 'pwd'
                stash includes: '**', name: 'sourceCode'
            }
        }
        stage('Maven Build') {
            agent {
                label 'myslave2'
            }
            steps {
                echo "Starting Maven build stage for ${params.NAME}"
                unstash 'sourceCode'
                sh 'mvn clean package'
                stash includes: 'target/*.jar', name: 'builtJar'
            }
        }
        stage('Docker Build and Push') {
            agent {
                label 'myslave2'
            }
            steps {
                echo "Starting Docker build and push stage for ${params.NAME}"
                unstash 'builtJar'
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials-id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh """
                            docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD your-private-registry-url
                            docker build -t your-private-registry-url/your-image-name:latest .
                            docker push your-private-registry-url/your-image-name:latest
                        """
                    }
                }
            }
        }
        stage('Docker Deploy') {
            agent {
                label 'myslave3'
            }
            steps {
                echo "Starting Docker deploy stage for ${params.NAME}"
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'deploy-ssh-credentials-id', keyFileVariable: 'SSH_KEY')]) {
                        sh """
                            ssh -i $SSH_KEY your-deploy-user@your-deploy-server '
                            docker pull your-private-registry-url/your-image-name:latest &&
                            docker stop your-image-name || true &&
                            docker rm your-image-name || true &&
                            docker run -d --name your-image-name -p 80:80 your-private-registry-url/your-image-name:latest
                            '
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }
        success {
            echo "Pipeline completed successfully for ${params.NAME}!"
        }
        failure {
            echo "Pipeline failed for ${params.NAME}!"
        }
    }
}
                sh 'ls -l'
                sh 'pwd'
            }
        }
        stage('Maven Build') {
            agent {
                label 'myslave2'
            }
            steps {
                echo "Starting Maven build stage for ${params.NAME}"
                sh 'mvn clean package'
            }
        }
        stage('Docker Build and Push') {
            agent {
                label 'myslave2'
            }
            steps {
                echo "Starting Docker build and push stage for ${params.NAME}"
                script {
                    docker.withRegistry("https://your-private-registry-url", 'docker-credentials-id') {
                        def image = docker.build("your-private-registry-url/your-image-name:latest")
                        image.push()
                    }
                }
            }
        }
        stage('Docker Deploy') {
            agent {
                label 'myslave3'
            }
            steps {
                echo "Starting Docker deploy stage for ${params.NAME}"
                script {
                    sshagent(['deploy-ssh-credentials-id']) {
                        sh """
                            ssh your-deploy-user@your-deploy-server '
                            docker pull your-private-registry-url/your-image-name:latest &&
                            docker stop your-image-name || true &&
                            docker rm your-image-name || true &&
                            docker run -d --name your-image-name -p 80:80 your-private-registry-url/your-image-name:latest
                            '
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }
        success {
            echo "Pipeline completed successfully for ${params.NAME}!"
        }
        failure {
            echo "Pipeline failed for ${params.NAME}!"
        }
    }
}
