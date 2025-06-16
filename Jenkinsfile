pipeline {
    agent any
    
    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        PUBLISH_DIR = "publish"
    }

    // Disable automatic SCM checkout since we'll do it manually
    options {
        skipDefaultCheckout true
    }

    stages {
        stage('Checkout') {
            steps {
                // Explicit checkout with branch specification
                git branch: 'main',
                     url: 'https://github.com/mohmedayman2009/dotnet-jenkins-demo.git'
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
                script {
                    try {
                        sh '''
                            sudo rm -rf /var/www/dotnetapp
                            sudo mkdir -p /var/www/dotnetapp
                            sudo cp -r $PUBLISH_DIR/* /var/www/dotnetapp/
                            echo "Deployment successful"
                        '''
                    } catch (err) {
                        echo "Deployment failed: ${err}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }
}
