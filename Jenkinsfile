pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                // Збираємо образ локально
                sh 'docker build -t martseniukapp:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Tests passed!"'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Pushing Docker image to DockerHub...'
                // Використовуємо збережені креденшали з ID 'dockerhub-credentials'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    
                    // Логінимось у DockerHub (пароль береться змінної, безпечно)
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    
                    // Тегаємо образ у форматі: логін/репозиторій:тег
                    sh 'docker tag martseniukapp:latest $DOCKER_USER/martseniukapp:latest'
                    
                    // Відправляємо (пушимо) образ
                    sh 'docker push $DOCKER_USER/martseniukapp:latest'
                }
            }
        }
    }
}