pipeline {
  agent any
  environment {
    IMAGE_NAME = 'alvynwira/simple-app'
    REGISTRY_CREDENTIALS = 'dockerhub-credentials' 
    GIT_URL = 'https://github.com/alvynpratama/jenkins-docker.git'
    GIT_CREDENTIALS = 'github-credentials' 
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: GIT_URL
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'echo "Membangun Docker Image..."'
        sh "docker build -t ${IMAGE_NAME} ."
      }
    }
    stage('Login & Push Docker Image') {
        steps {
            sh 'echo "Pushing Docker Image ke Docker Hub..."'
            withCredentials([usernamePassword(credentialsId: REGISTRY_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                sh "docker push ${IMAGE_NAME}"
            }
        }
    }
    stage('Push Folder ke GitHub') {
        steps {
            withCredentials([string(credentialsId: GIT_CREDENTIALS, variable: 'GIT_TOKEN')]) {
                script {
                    sh '''
                      git config --global user.email "jenkins-bot@example.com"
                      git config --global user.name "Jenkins Bot"
                      git add .
                      
                      # Cek dulu apakah ada perubahan, agar tidak error jika tidak ada commit baru
                      if ! git diff-index --quiet HEAD; then
                        echo "Mendeteksi perubahan, melakukan commit..."
                        git commit -m "Push updates from Jenkins [ci skip]"
                        
                        # Set remote URL dengan token untuk autentikasi push
                        git remote set-url origin https://${GIT_TOKEN}@github.com/alvynpratama/jenkins-docker.git
                        
                        git push origin main
                      else
                        echo "Tidak ada perubahan untuk di-push ke GitHub."
                      fi
                    '''
                }
            }
        }
    }
  }
  post {
    always {
      echo 'Proses selesai.'
      sh 'docker logout'
    }
  }
}
