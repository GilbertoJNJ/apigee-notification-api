pipeline {
    agent any

    environment {
        APIGEE_ORG = 'your-apigee-org'
        APIGEE_ENV = 'your-apigee-env'
        APIGEE_API = 'notifications'
        APIGEE_USERNAME = credentials('apigee-username')
        APIGEE_PASSWORD = credentials('apigee-password')
        APIGEE_BASE_URL = 'https://api.enterprise.apigee.com/v1'
    }

    stages {
        stage('Checkout') {
            steps {
                // Obtém o código do repositório
                git branch: 'main', url: 'https://your-git-repo-url.git'
            }
        }

        stage('Build Proxy') {
            steps {
                // Navega até o diretório do proxy e compacta em um arquivo ZIP
                dir('src/main/apigee/apiproxies/notification-api') {
                    sh 'zip -r notifications.zip apiproxy/'
                }
            }
        }

        stage('Deploy Proxy') {
            steps {
                // Faz o deploy do proxy no Apigee
                script {
                    withCredentials([usernamePassword(credentialsId: 'apigee-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh """
                        curl -X POST "$APIGEE_BASE_URL/organizations/$APIGEE_ORG/apis?action=import&name=$APIGEE_API" \
                        -u $USERNAME:$PASSWORD \
                        -F "file=@src/main/apigee/apiproxies/notification-api/notifications.zip" \
                        -F "name=$APIGEE_API"

                        curl -X POST "$APIGEE_BASE_URL/organizations/$APIGEE_ORG/environments/$APIGEE_ENV/apis/$APIGEE_API/revisions/1/deployments" \
                        -u $USERNAME:$PASSWORD
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Proxy successfully deployed!'
        }
        failure {
            echo 'Proxy deployment failed.'
        }
    }
}
