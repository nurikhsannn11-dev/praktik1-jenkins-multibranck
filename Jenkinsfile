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
            // Mengirim notifikasi jika build SUKSES menggunakan Discord Notifier
            discordSend(
                webhookURL: 'https://ptb.discord.com/api/webhooks/1425392944931672106/SL865yKe8A0YkqtZF2igdoStSLDZK4QRsEF2hvaZ3iIdbsE7EYiKvS-IPWGgX_ElmUf0',
                title: 'Build Succeeded',
                message: "✅ Build SUKSES: Job `${env.JOB_NAME}` pada branch `${env.BRANCH_NAME}` berhasil.",
                color: '#00ff00',
                link: env.BUILD_URL
            )
        }
        failure {
            // Mengirim notifikasi jika build GAGAL menggunakan Discord Notifier
            discordSend(
                webhookURL: 'https://ptb.discord.com/api/webhooks/1425392944931672106/SL865yKe8A0YkqtZF2igdoStSLDZK4QRsEF2hvaZ3iIdbsE7EYiKvS-IPWGgX_ElmUf0',
                title: 'Build Failed',
                message: "❌ Build GAGAL: Job `${env.JOB_NAME}` pada branch `${env.BRANCH_NAME}` gagal.",
                color: '#ff0000',
                link: env.BUILD_URL
            )
        }
    }
}
