pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root'  // Menjalankan kontainer sebagai root untuk menghindari masalah izin
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Pastikan file 'requirements.txt' ada di repositori Anda
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Pastikan 'pytest' ada di requirements.txt dan file 'test_app.py' ada
                sh 'pytest test_app.py'
            }
        }

        stage('Deploy') {
            when {
                // Stage ini hanya berjalan di branch 'main' atau branch 'release/*'
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
    success {
        discordSend(
            webhookURL: 'https://ptb.discord.com/api/webhooks/1425392944931672106/SL865yKe8A0YkqtZF2igdoStSLDZK4QRsEF2hvaZ3iIdbsE7EYiKvS-IPWGgX_ElmUf0', // URL webhook Anda
            title: "✅ Build SUCCESS on ${env.BRANCH_NAME}",
            description: "URL: ${env.BUILD_URL}",
            color: '#00ff00'
        )
    }
    failure {
        discordSend(
            webhookURL: 'https://ptb.discord.com/api/webhooks/1425392944931672106/SL865yKe8A0YkqtZF2igdoStSLDZK4QRsEF2hvaZ3iIdbsE7EYiKvS-IPWGgX_ElmUf0', // URL webhook Anda
            title: "❌ Build FAILED on ${env.BRANCH_NAME}",
            description: "URL: ${env.BUILD_URL}",
            color: '#ff0000'
        )
    }
    always {
        // Membersihkan workspace setelah build selesai
        cleanWs()
    }
  }
}
