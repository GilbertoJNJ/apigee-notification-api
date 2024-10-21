pipeline {
    agent any

    environment {
        APIGEE_ORG = 'blissful-axiom-358022'
        APIGEE_ENV = 'eval'
        APIGEE_API = 'notifications'
        APIGEE_BASE_URL = 'https://api.enterprise.apigee.com/v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature/deploy-com-jenkins', url: 'https://github.com/GilbertoJNJ/apigee-notification-api.git'
            }
        }

        stage('Build Proxy') {
            steps {
                dir('src/main/apigee/apiproxies/notification-api') {
                    bat 'tar -cvf notifications.zip apiproxy/*'
                }
            }
        }

        stage('Deploy Proxy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'apigee-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        bat '''
                        curl -X POST "$APIGEE_BASE_URL/organizations/$APIGEE_ORG/apis?action=import&name=$APIGEE_API" \
                        -u $USERNAME:$PASSWORD \
                        -F "file=@src/main/apigee/apiproxies/notification-api/notifications.zip" \
                        -F "name=$APIGEE_API"

                        curl -X POST "$APIGEE_BASE_URL/organizations/$APIGEE_ORG/environments/$APIGEE_ENV/apis/$APIGEE_API/revisions/1/deployments" \
                        -u $USERNAME:$PASSWORD
                        '''
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
