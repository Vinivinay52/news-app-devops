pipeline {
    agent { label 'slave20' }

    environment {
        TOMCAT_PATH = "/opt/tomcat10/webapps"
        WAR_FILE = "target/news-app.war"
        ARTIFACTORY_URL = "https://trialdoenfo.jfrog.io/artifactory"
        ARTIFACTORY_REPO = "newsapp_release"
        ARTIFACT_NAME = "news-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'feature-1', url: 'https://github.com/Vinivinay52/news-app-devops.git'
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Push Artifact to JFrog Artifactory') {
            steps {
                script {
                    // Timestamp version e.g., 2025-12-04_19-45
                    def currentDate = new java.text.SimpleDateFormat("yyyy-MM-dd_HH-mm").format(new Date())
                    def version = currentDate
                    echo "Generated Version: ${version}"

                    // Use Jenkins credential ID
                    withCredentials([usernamePassword(credentialsId: 'jfrog-creds',
                                                     usernameVariable: 'ART_USERNAME',
                                                     passwordVariable: 'ART_PASSWORD')]) {
                        sh """
                            curl -u $ART_USERNAME:$ART_PASSWORD -T $WAR_FILE \
                            "$ARTIFACTORY_URL/$ARTIFACTORY_REPO/${ARTIFACT_NAME}-${version}.war"
                        """
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    sh """
                        sudo rm -rf $TOMCAT_PATH/news-app*
                        sudo cp $WAR_FILE $TOMCAT_PATH/
                        sudo systemctl restart tomcat
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Build & Deployment completed successfully!"
        }
        failure {
            echo "Pipeline failed. Please check logs!"
        }
    }
}

