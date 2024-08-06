pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/spasham/kubernetesmanifest.git'
        GIT_BRANCH = 'main'
        GIT_CREDENTIALS_ID = 'github' // Replace with your Jenkins credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[url: "${env.GIT_REPO}", credentialsId: "${env.GIT_CREDENTIALS_ID}"]]
                ])
            }
        }

        stage('Commit Changes') {
            steps {
                sh 'git commit -m "Done by Jenkins Job changemanifest: 5"'
            }
        }

        stage('Push Changes') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${env.GIT_CREDENTIALS_ID}", passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        git config --global user.email "jenkins@example.com"
                        git config --global user.name "Jenkins"
                        git pull https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/spasham/kubernetesmanifest.git ${env.GIT_BRANCH} --allow-unrelated-histories
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/spasham/kubernetesmanifest.git HEAD:${env.GIT_BRANCH}
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            // Add any cleanup steps here
        }
        failure {
            // Add any steps to take on failure here
        }
    }
}

