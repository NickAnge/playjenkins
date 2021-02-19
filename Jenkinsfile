pipeline {

    environment {
        registry = "192.168.1.189:5000/nickange/myweb"
        dockerImage = ""
    }

    agent any

    stages {

        stage('Checkout Source') {
            steps {
                git "https://github.com/NickAnge/playjenkins.git"
            }
        }

        stage('Build Image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage("Push Image") {
            steps{
                script {
                    docker.withRegistry( "" ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy App') {
            steps {
                script {
                    kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
                }
            }
        }
        
    }
}
