pipeline {
    agent { label 'jenkins-slave' }  // Runs on the slave node
    parameters {
        choice(name: 'ENV_GROUP', choices: ['main','dev'], description: 'Environment to deploy the VMs')
        booleanParam(name: 'GENERATE_DOCS', defaultValue: false, description: 'Should the docs be generated?')
        booleanParam(name: 'ONLY_ENV', defaultValue: false, description: 'Only update secrets?')
    }


    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    docker build -t main-backend-jenkins-direct:v1 .
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh '''
                    # Stop and remove the existing container if running
                    docker stop main-backend-jenkins-direct || true
                    docker rm main-backend-jenkins-direct || true

                    # Run the new container
                    docker run -d -p 80:80 --name main-backend-jenkins-direct main-backend-jenkins-direct:v1
                    '''
                }
            }
        }
    }
}