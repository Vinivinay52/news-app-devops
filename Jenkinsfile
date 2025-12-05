pipeline {
    agent { label 'slave20' }

    environment {
        TOMCAT_PATH = "/opt/tomcat10/webapps"
        WAR_FILE = "target/news-app.war"
        ARTIFACTORY_SERVER = "my-artifactory"
        REPO_KEY = "newsapp-release"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'feature-1', url: 'https://github.com/Vinivinay52/news-app-devops.git'
            }
        }

        stage('Version & Build') {
            steps {
                script {
                    def version = "1.0.${env.BUILD_NUMBER}"
                    echo "Setting project version to ${version}"
                    sh """
                        mvn versions:set -DnewVersion=${version}
                        mvn clean package
                    """
                }
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Artifactory Config') {
            steps {
                script {
                    rtServer(
                        id: ARTIFACTORY_SERVER,
                        url: 'https://trialeysrup.jfrog.io/artifactory',
                        credentialsId: 'jfrog-cred'
                    )
                }
            }
        }

  stage('Upload Artifact to JFrog') {
    steps {
        script {
            def server = Artifactory.server('artifactory-cred')
            def buildInfo = Artifactory.newBuildInfo()

            server.upload(
                spec: """{
                    "files": [
                        {
                            "pattern": "target/*.war",
                            "target": "newsapp-release/"
                        }
                    ]
                }""",
                buildInfo: buildInfo
            )
            
            server.publishBuildInfo(buildInfo)
        }
    }
}


        stage('Deploy to Tomcat') {
            steps {
                sh "sudo rm -rf ${TOMCAT_PATH}/news-app.war"
                sh "sudo cp ${WAR_FILE} ${TOMCAT_PATH}/"
            }
        }
    }
}
