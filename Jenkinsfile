pipeline {
    agent { label 'slave5' }
    stages {
        stage('Checkout') {
            steps {
                sh "rm -rf news-app-devops "
                sh "https://github.com/Vinivinay52/news-app-devops.git"
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Deploy') {
            steps {
                sh "sudo rm -rf /opt/tomcat10/webapps/news-app.war"
                sh "sudo cp /home/ubuntu/news-app-devops/target/news-app.war /opt/tomcat10/webapps"
            }
        }
    }
}

