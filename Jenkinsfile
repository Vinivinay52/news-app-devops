pipeline {
    agent { label 'slave1' }
    stages {
        stage('checkout') {
            steps {
                sh "rm -rf news-app-devops "
                sh "git clone https://github.com/Manselariqwxd3/news-app-devops"
            }
        }
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('Deploy') {
            steps {
                sh "sudo rm -rf /opt/tomcat10/webapps/news-app.war"

                sh "sudo cp /home/ubuntu/workspace/news-app-devops_feature-1/target/news-app.war /opt/tomcat10/webapps"
            }
        }
    }
}
