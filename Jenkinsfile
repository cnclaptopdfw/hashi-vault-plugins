pipeline {
  agent any

  environment {
    GO_VERSION = '1.21.0' // Adjust based on Vault's compatibility
    VAULT_PLUGIN_NAME = 'my-custom-plugin'
    VAULT_PLUGIN_PATH = "vault-custom-plugin"
    OUTPUT_DIR = 'bin'
  }

  stages {
    stage('Setup Go') {
      steps {
        sh 'go version || echo "Go not found. Make sure it is installed on this agent."'
      }
    }

    stage('Checkout Code') {
      steps {
        // Optionally clone from Git
        // git url: 'https://github.com/your-org/vault-custom-plugin.git'
        echo "Assuming code is already present in workspace"
      }
    }

    stage('Build Plugin') {
      steps {
        dir("${VAULT_PLUGIN_PATH}") {
          sh """
            mkdir -p ../${OUTPUT_DIR}
            go build -o ../${OUTPUT_DIR}/${VAULT_PLUGIN_NAME}
          """
        }
      }
    }

    stage('Checksum (optional)') {
      steps {
        sh """
          sha256sum ${OUTPUT_DIR}/${VAULT_PLUGIN_NAME} > ${OUTPUT_DIR}/${VAULT_PLUGIN_NAME}.sha256
        """
      }
    }

    stage('Upload/Deploy (optional)') {
      steps {
        echo "You could upload to S3, move to Vault plugins dir, etc."
        // e.g., sh "mv ${OUTPUT_DIR}/${VAULT_PLUGIN_NAME} /etc/vault/plugins/"
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: "${OUTPUT_DIR}/*", fingerprint: true
    }
  }
}
