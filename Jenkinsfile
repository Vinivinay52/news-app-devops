pipeline {
    agent { label 'java' }

    stages {
        stage('News-App-Checkout') {
            steps {
                sh 'rm -rf news-app-devops'
                sh 'git clone https://github.com/Vinivinay52/news-app-devops.git'
                echo "git clone completed"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Version-Build') {
            steps {
                script {
                    def version = "1.0.${env.BUILD_NUMBER}"
                    echo "Setting project version to ${version}"

                    sh "mvn versions:set -DnewVersion=${version}"
                    sh "mvn clean package"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "sudo cp news-app-devops/target/news-app.war /opt/tomcat10/webapps/"
                echo "build deployed"
            }
        }

        stage('Push the artifacts into Jfrog Artifactory') {
            steps {
                script {
                    def currentDate = new java.text.SimpleDateFormat("yyyy-MM-dd_HH-mm").format(new Date())
                    def targetPath = "NewsApp/${currentDate}/"

                    rtServer(
                        id: 'Artifactory',
                        url: 'https://trialyth1ui.jfrog.io/artifactory',
                        credentialsId: 'jfrog-credentials-id'
                    )

                    rtUpload(
                        serverId: 'Artifactory',
                        spec: """{
                            "files": [{
                                "pattern": "news-app-devops/target/*.war",
                                "target": "${targetPath}"
                            }]
                        }"""
                    )
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'news-app-devops/target/*.war', fingerprint: true
        }
    }
}

