pipeline {
environment {
    JFROG_URL = 'https://trialeysrup.jfrog.io/artifactory'
    REPO_NAME = 'news_app-libs-snapshot'      // JFrog repo for feature branches
	 
  }
    agent { label 'slave1' }
    stages {
        stage('Checkout') {
            steps {
                 sh "rm -rf news-app-devops"
               sh "https://github.com/Vinivinay52/news-app-devops.git"
            }
        } 
        stage('Build') {
            steps {
           sh '''
		   sudo apt-get update
		   sudo apt install -y maven
				 mvn clean package
				 '''
            }
        } 
        
		
    //    stage('Create Versioned Artifact') {
    //   steps {
    //     script {
    //       def sha = sh(
    //         script: 'git rev-parse --short HEAD',
    //         returnStdout: true
    //       ).trim()

    //       def branchSafe = env.BRANCH_NAME.replaceAll('[^a-zA-Z0-9_.-]', '_')

    //       env.ARTIFACT = "bus_booking-${branchSafe}-${env.BUILD_NUMBER}-${sha}.war"
        
    //       sh "cp /home/ubuntu/workspace/bus_booking_feature-1/target/bus-booking-app-1.0-SNAPSHOT.war ${env.ARTIFACT}"
    //       archiveArtifacts artifacts: "${env.ARTIFACT}", fingerprint: true
    //     }
    //   }
    // }

    // stage('Upload to JFrog') {
    //   steps {
    //     withCredentials([string(credentialsId: 'jfrogkey', variable: 'JFROG_API_KEY')]) {
    //       sh """
    //         curl -f -H "X-JFrog-Art-Api: ${JFROG_API_KEY}" \
    //             -T "${env.ARTIFACT}" \
    //             "${JFROG_URL}/${REPO_NAME}/${env.BRANCH_NAME}/${env.ARTIFACT}"
    //       """
    
    //   }
    // }
       
	//	}
// 		 stage('Deploy') {
//     steps {
          
//         sh "sudo cp  /home/ubuntu/workspace/bus_booking_feature-1/target/bus-booking-app-1.0-SNAPSHOT.war /opt/tomcat10_9090/webapps"
// 		sh "sudo /opt/tomcat10_9090/bin/startup.sh"
//          }
// }
 }

}
