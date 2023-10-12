pipeline {
  agent {
  kubernetes {}
  }
  stages {
        stage('Clone Repo') {
            steps {
		git branch: "${BRANCH_NAME}",
		    url: 'https://github.com/mtsantosd/to_k8s.git'

		sh "ls -lat"
            }
        }
        stage('Deploy app on K8s') {
            steps {
                withKubeConfig([credentialsId: '80a643d4-6238-45be-b0a9-c9857a5133fe']) {
                    sh 'curl -LO https://dl.k8s.io/release/v1.28.2/bin/linux/amd64/kubectl'
                    sh 'chmod +x kubectl'
                    sh 'mkdir -p ~/.local/bin'
                    sh 'mv ./kubectl ~/.local/bin/kubectl'
                    sh '/home/jenkins/.local/bin/kubectl version --client'
                    sh '/home/jenkins/.local/bin/kubectl get ns ${env.BRANCH_NAME} || /home/jenkins/.local/bin/kubectl create ns ${env.BRANCH_NAME}"'
                    sh 'sleep 20'
                    sh '/home/jenkins/.local/bin/kubectl get pods,service -n web-"${env.BRANCH_NAME}"'
                }
            }
        }
    }
}
