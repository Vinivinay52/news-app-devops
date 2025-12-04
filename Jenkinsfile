pipeline {
    agent { label 'slave20' }

    environment {
        TOMCAT_PATH = "/opt/tomcat10/webapps"
        WAR_FILE = "target/news-app.war"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature-2', url: 'https://github.com/Vinivinay52/news-app-devops.git'
            }
        }

        stage('Build') {
            steps {
                // This runs maven package (will run tests because -DskipTests=false)
                sh 'mvn clean package -DskipTests=false'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }


stage('Push the artifacts into JFrog Artifactory') {
    steps {
        script {
            def currentDate = new java.text.SimpleDateFormat("yyyy-MM-dd_HH-mm").format(new Date())
            def repo = "newsapp-snapshots-local"       // <-- your REAL repo key

            rtUpload(
                serverId: "jfrog",
                spec: """{
                    "files": [
                        {
                            "pattern": "${WAR_FILE}",
                            "target": "${repo}/newsapp/${currentDate}/news-app.war"
                        }
                    ]
                }"""
            )
        }
    }
}


        
        stage('Deploy WAR to Tomcat') {
            steps {
                sh(script: '''
                    echo "Using TOMCAT_PATH=${TOMCAT_PATH}"
                    echo "WAR_FILE=${WAR_FILE}"

                    if [ ! -f "${WAR_FILE}" ]; then
                      echo "ERROR: WAR file ${WAR_FILE} not found"
                      exit 1
                    fi

                    echo "Cleaning old deployment..."
                    sudo rm -rf "${TOMCAT_PATH}/news-app" "${TOMCAT_PATH}/news-app.war" || true

                    echo "Copying new WAR..."
                    sudo cp "${WAR_FILE}" "${TOMCAT_PATH}/"

                    echo "Restarting Tomcat..."
                    # kill existing Tomcat process if running
                    pkill -f 'org.apache.catalina.startup.Bootstrap' || true

                    # start Tomcat (assumes ../bin/startup.sh is the startup script)
                    nohup "${TOMCAT_PATH}/../bin/startup.sh" > /dev/null 2>&1 &
                ''')
            }
        }
}
}
