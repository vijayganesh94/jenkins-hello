pipeline {
  agent any
  options { timestamps() }
  stages {
    stage('Run script') {
      steps {
        sh 'chmod +x hello.sh'
        // capture output to a file and also show in console
        sh './hello.sh | tee output.txt'
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'output.txt', fingerprint: true
    }
    success {
      // requires Email Extension Plugin
      emailext(
        to: 'imvijay594@gmail.com',
        subject: "Jenkins ✅ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build URL: ${env.BUILD_URL}

<pre>
${BUILD_LOG, maxLines=200}
</pre>

Output file contents:
${FILE, path="output.txt"}
""",
        attachmentsPattern: 'output.txt'
      )
    }
    failure {
      emailext(
        to: 'imvijay594@gmail.com',
        subject: "Jenkins ❌ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build failed.

Build URL: ${env.BUILD_URL}

<pre>
${BUILD_LOG, maxLines=200}
</pre>
"""
      )
    }
  }
}
