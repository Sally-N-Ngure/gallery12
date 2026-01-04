pipeline {
  agent any

  tools {
    nodejs "NodeJS18"
  }

  triggers {
    githubPush()
  }

  environment {
    MONGODB_URI = credentials("IP1-MongoDB")
    RENDER_BASE_URL = "https://gallery-45sh.onrender.com/"
  }

  stages {
    stage("Checkout") {
      steps {
        checkout scm
      }
    }

    stage("Install Dependencies") {
      steps {
        sh "npm ci"
      }
    }

    stage("Run Tests") {
      steps {
        sh "npm test"
      }
      post {
        failure {
          emailext(
            subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "The build for ${env.JOB_NAME} #${env.BUILD_NUMBER} has failed. Please check the Jenkins console output for details.",
            to: "douglas.wangome@student.moringaschool.com"
          )
        }
      }
    }

    stage("Deploy to Render") {
      steps {
        echo "Triggering deployment to Render..."
        withCredentials([string(credentialsId: 'RenderDeployHook', variable: 'RENDER_HOOK')]) {
          sh "curl -s -X POST ${RENDER_HOOK}"
        }
      }
      post {
        success {
          slackSend(
            channel: "#YourFirstName_IP1",
            color: "good",
            message: "Build and deployment successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Site: ${RENDER_BASE_URL}",
            teamDomain: "moringadevops12",
            tokenCredentialId: "SlackJenkins",
            botUser: true
          )
        }
        failure {
          slackSend(
            channel: "#YourFirstName_IP1",
            color: "danger",
            message: "Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}. Check Jenkins logs.",
            teamDomain: "moringadevops12",
            tokenCredentialId: "SlackJenkins",
            botUser: true
          )
        }
      }
    }
  }

  post {
    always {
      script {
        if (currentBuild.result == 'SUCCESS') {
          echo "Build completed successfully"
        } else {
          echo "Build failed, check the logs for details"
        }
      }
    }
  }
}