pipeline {
    agent { label 'slave20' }

    environment {
        TOMCAT_PATH = "/opt/tomcat10/webapps"
        WAR_FILE = "target/news-app.war"
		JFROG_URL = 'https://trialeysrup.jfrog.io/artifactory'
    REPO_NAME = 'news_app-libs-snapshot'      // JFrog repo for feature branches.
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

		 stage('Deploy') {
    steps {
          
        sh "sudo cp  /home/slave20/workspace/news-app-devops_job1_feature-1/target/news-app.war /opt/tomcat10/webapps"
	
         }
}


       stage('Create Versioned Artifact') {
      steps {
        script {
          def sha = sh(
            script: 'git rev-parse --short HEAD',
            returnStdout: true
          ).trim()

          def branchSafe = env.BRANCH_NAME.replaceAll('[^a-zA-Z0-9_.-]', '_')

          env.ARTIFACT = "bus_booking-${branchSafe}-${env.BUILD_NUMBER}-${sha}.war"
        
          sh "cp /home/slave20/workspace/news-app-devops_job1_feature-1/target/news-app.war ${env.ARTIFACT}"
          archiveArtifacts artifacts: "${env.ARTIFACT}", fingerprint: true
        }
      }
    }

    stage('Upload to JFrog') {
      steps {
        withCredentials([string(credentialsId: 'JFROG_API_KEY', variable: 'JFROG_API_KEY')]) {
          sh """
            curl -f -H "X-JFrog-Art-Api: ${JFROG_API_KEY}" \
                -T "${env.ARTIFACT}" \
                "${JFROG_URL}/${REPO_NAME}/${env.BRANCH_NAME}/${env.ARTIFACT}"
          """
    
      }
    }
       
		}
}
}
