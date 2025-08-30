pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Run script') {
      steps {
        sh 'chmod +x hello.sh'
        // run script and capture output to file
        sh './hello.sh | tee output.txt'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'output.txt', fingerprint: true
    }

    success {
      script {
        // Capture last 100 lines of console log safely
        def logContent = currentBuild.rawBuild.getLog(100).join("\n")

        emailext(
          to: 'imvijay594@gmail.com',
          subject: "Jenkins ✅ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
          body: """Build succeeded!

Build URL: ${env.BUILD_URL}

Last 100 lines of log:
----------------------------------------
${logContent}
----------------------------------------

Output file contents:
${readFile('output.txt')}
""",
          attachmentsPattern: 'output.txt'
        )
      }
    }

    failure {
      script {
        def logContent = currentBuild.rawBuild.getLog(100).join("\n")

        emailext(
          to: 'imvijay594@gmail.com',
          subject: "Jenkins ❌ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
          body: """Build failed.

Build URL: ${env.BUILD_URL}

Last 100 lines of log:
----------------------------------------
${logContent}
----------------------------------------
"""
        )
      }
    }
  }
}
