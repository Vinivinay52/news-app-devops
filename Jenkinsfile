pipeline {
    agent { label 'slave1' }
    stages {
        stage('Checkout') {
            steps {
                sh "rm -rf news-app-devops "
                sh "git clone https://github.com/ManasaaMarigowda/news-app-devops"
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
                sh "sudo cp /home/ubuntu/workspace/news-app-devops_feature-1/target/news-app.war /opt/tomcat10/webapps"
