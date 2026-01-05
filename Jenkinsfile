pipeline {
  agent any

  tools {
    nodejs "NodeJS18"
  }

  environment {
    MONGODB_URI = credentials("IP1-MongoDB")
    RENDER_BASE_URL = "https://gallery-45sh.onrender.com/"
  }

  stages {
    stage("Clone Repository") {
      steps {
        git branch: "master", url: "https://github.com/Sally-N-Ngure/gallery12.git"
      }
    }

    stage("Install Dependecies") {
      steps {
        sh "npm ci"
      }
    }

    stage("Run Tests") {
      steps {
        sh "npm test"
      }

    //   post {
    //     failure {
    //       emailext(
    //         subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
	// 					body: "The build for ${env.JOB_NAME} #${env.BUILD_NUMBER} has failed. Please check the Jenkins console output for more details.",
	// 					to: "douglas.wangome@student.moringaschool.com"
    //       )
    //     }
    //   }
    }

//     stage("Deploy to Render") {
//       steps {
//         echo "Deploying to Render..."
// 				echo "Triggering deployment to Render with base URL: ${RENDER_BASE_URL}"
// 				echo "Deployment complete."
//       }

//       post {
//         success{
//           slackSend(
// 						channel: "#douglas_ip1",
// 						color: "good",
// 						message: "Build and deployment successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Check the build at ${RENDER_BASE_URL}",
// 						teamDomain: "moringadevops10",
// 						tokenCredentialId: "Jenkins-App-Slack",
// 						botUser: true
// 					)
//         }
//         failure{
//           slackSend(
// 						channel: "#douglas_ip1",
// 						color: "danger",
// 						message: "Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Please check the Jenkins console output for more details.",
// 						teamDomain: "moringadevops10",
// 						tokenCredentialId: "Jenkins-App-Slack",
// 						botUser: true
// 					)
//         }
//       }
//     }
//   }

//   post {
//     always {
//       script {
//         if (currentBuild.result == 'SUCCESS') {
// 					echo "Build completed successfully"
// 				} else {
// 					echo "Build failed, check the logs for details"
// 				}
//       }
//     }
  }
}