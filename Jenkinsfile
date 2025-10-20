pipeline {
  agent any
  environment {
    IMAGE_NAME = 'alvynwira/simple-app'  // Ganti dengan nama pengguna Docker Hub dan image yang ingin digunakan
    REGISTRY = 'https://index.docker.io/v1/'  // Docker Hub registry
    REGISTRY_CREDENTIALS = 'dockerhub-credentials'  // Nama ID kredensial yang sudah disimpan
    GIT_URL = 'https://github.com/alvynpratama/jenkins-docker.git'  // Ganti dengan URL GitHub repo Anda
    GIT_CREDENTIALS = 'github-credentials'  // Ganti dengan nama ID kredensial GitHub yang sudah disimpan
  }
  stages {
    stage('Checkout') {
      steps {
        git credentialsId: GIT_CREDENTIALS, url: GIT_URL
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'echo "Membangun Docker Image"'
      }
    }
    stage('Push Folder ke GitHub') {
      steps {
        script {
          sh '''
            git config --global user.email "youremail@example.com"  // Ganti dengan email GitHub kamu
            git config --global user.name "alvynwira"  // Nama GitHub kamu
            git add .  // Menambahkan semua perubahan
            git commit -m "Push updates from Jenkins"  // Commit perubahan
            git push origin main  // Push ke branch utama di GitHub (ganti 'main' jika menggunakan branch lain)
          '''
        }
      }
    }
  }
  post {
    always {
      echo 'Proses selesai.'
    }
  }
}
