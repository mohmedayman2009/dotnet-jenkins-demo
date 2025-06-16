pipeline {
    agent any

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        PUBLISH_DIR = "publish"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/mohmedayman2009/dotnet-jenkins-demo.git', branch: 'main'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o $PUBLISH_DIR'
            }
        }

        stage('Deploy to NGINX') {
            steps {
                sh '''
                    sudo rm -rf /var/www/dotnetapp
                    sudo mkdir -p /var/www/dotnetapp
                    sudo cp -r $PUBLISH_DIR/* /var/www/dotnetapp/
                '''
            }
        }
    }
}
