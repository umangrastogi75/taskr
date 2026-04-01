pipeline {
    agent any // Run the pipeline on any available agent
    stages {
        stage('Build') {
            steps {
                echo 'Building..' 
                sh 'docker compose up -d'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'

            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                
            }
        }
    }
}
