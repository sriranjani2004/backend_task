pipeline {
    agent any
    tools {
        nodejs 'NodeJS'  
    }
    
    environment {
        NODEJS_HOME = '/usr/local/bin/node'
        SONAR_SCANNER_PATH = '/Users/ariv/Downloads/sonar-scanner-6.2.1.4610-macosx-x64/bin/sonar-scanner'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install and Build') {
            steps {
                sh '''npm install
                npm run lint'''  
            }
        }

        
        stage('SonarCodeAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')  
            }
            steps {
                sh '''
                export PATH=$SONAR_SCANNER_PATH:$PATH
                which sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=newprojectbackend \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=$SONAR_TOKEN
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESSFULLY Build"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
