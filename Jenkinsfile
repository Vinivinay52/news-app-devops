pipeline {
  agent { label 'slave20' }
	environment {
    JFROG_URL = 'https://trialeysrup.jfrog.io/artifactory'
    REPO_NAME = 'newsapp_release'      // JFrog repo for feature branches
  }
	
 stages {
	 stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
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

          env.ARTIFACT = "news-app-${branchSafe}-${env.BUILD_NUMBER}-${sha}.war"

          sh "cp target/*.war ${env.ARTIFACT}"
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
	 
    stage('Deploy to Tomcat') {
      steps {
			sh "sudo rm -rf /opt/tomcat10/webapps/news-app"
			//sudo rm /opt/tomcat10/webapps/news-app.war
			sh "sudo cp /home/ubuntu/news-app-devops/target/news-app.war /opt/tomcat10/webapps"
		  		  	sh "sudo /opt/tomcat10/bin/shutdown.sh"
			sh "sudo /opt/tomcat10/bin/startup.sh"
        }
      }
    }
}
