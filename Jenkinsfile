pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Run script') {
      steps {
        sh 'chmod +x hello.sh'
        // Save output into a file
        sh './hello.sh | tee output.txt | tee build.log'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '*.txt,*.log', fingerprint: true
    }

    success {
      emailext(
        to: 'imvijay594@gmail.com',
        subject: "Jenkins ✅ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build succeeded!

Build URL: ${env.BUILD_URL}

Script Output:
----------------------------------------
${readFile('output.txt')}
----------------------------------------

Console Log (last lines):
----------------------------------------
${readFile('build.log')}
----------------------------------------
""",
        attachmentsPattern: '*.txt,*.log'
      )
    }

    failure {
      emailext(
        to: 'imvijay594@gmail.com',
        subject: "Jenkins ❌ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build failed.

Build URL: ${env.BUILD_URL}

See attached log files for details.
""",
        attachmentsPattern: '*.txt,*.log'
      )
    }
  }
}
