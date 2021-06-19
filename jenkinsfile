pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'hasher/Dockerfile'
                       }
            }
            steps {
                sh 'ruby --version'
            }
        }
    }
}
