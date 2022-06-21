pipeline {
    agent any
    
    stages {
        stage ('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("rafanfortes/api-produto:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
        
            }
        }

        stage ('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ('Deploy Kubernetes') {
            steps {
                withKubeConfig([credentialIds: 'kubeconfig']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }

    }
}