@Library('Shared') _

pipeline {
    agent { label "Lakshay" }
    
    stages {
        stage("Install Docker Compose") {
            steps {
                sh """
                sudo apt-get update
                sudo apt-get install -y curl
                sudo curl -L "https://github.com/docker/compose/releases/download/v2.21.0/docker-compose-\$(uname -s)-\$(uname -m)" -o /usr/local/bin/docker-compose
                sudo chmod +x /usr/local/bin/docker-compose
                """
            }
        }
        
        stage("Verify Docker Compose") {
            steps {
                sh "docker-compose --version"
            }
        }
        
        stage("Hello") {
            steps {
                script {
                    hello() // Ensure 'hello()' is defined in your shared library
                }
            }
        }
        
        stage("Clone Repository") {
            steps {
                script {
                    clone("https://github.com/Laksh-31/django-notes-app.git", "main") // Ensure 'clone()' is defined
                }
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    dockerbuild("notes-app", "latest", "laksh31") // Ensure 'dockerbuild()' is defined
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    dockerpush("notes-app", "latest", "laksh31") // Ensure 'dockerpush()' is defined
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the Docker containers using Docker Compose"
                sh "docker-compose up -d"
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
