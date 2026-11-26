pipeline{
    agent { label 'node-agent' }
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/manapanda1/node-todo-cicd.git',branch:'master'
            }
        }
        stage('Build & Test'){
            steps{
                sh 'docker build . -t manapanda/node-todo-test:latest'
            }
        }
         stage('Push') {
    steps {
        script {
            withCredentials([usernamePassword(
                credentialsId: 'dockerHub',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {
                sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push manapanda/node-todo-test:latest
                    docker logout
                '''
            }
        }
    }
}
       
        stage('Deploy'){
            steps{
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}