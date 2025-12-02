pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kristinapetkova-ship-it/HW.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-netlify-agent .'
            }
        }

        stage('Deploy to Netlify') {
            steps {
                sh '''
                docker run --rm \
                  -e NETLIFY_AUTH_TOKEN=$NETLIFY_AUTH_TOKEN \
                  -v $PWD:/workspace \
                  jenkins-netlify-agent \
                  sh -c "netlify deploy --prod --dir=/workspace --auth=$NETLIFY_AUTH_TOKEN --site=55b7d791-24fa-46d7-abde-55b7f6292ac9"
                '''
            }
        }
    }
}