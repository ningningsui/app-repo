Gpipeline {
    agent {
        label 'gitops-jenkins-jenkins-slave'
    }
    stages {
        stage('Get Source') {
            steps {
                echo "1.Clone Repo Stage"
                git credentialsId: 'GitHubAccess', url: 'https://github.com/ningningsui/app-repo'
                script {
                    build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    repo_name = '492666533052.dkr.ecr.ap-south-1.amazonaws.com'
                    app_name = 'gitops-app-demo'
                }
            }
        }
        stage('Build Image') {
            steps {
                echo "2.Build Docker Image Stage"
                sh "docker build --network host -t ${repo_name}/${app_name}:latest ."
                sh "docker tag ${repo_name}/${app_name}:latest ${repo_name}/${app_name}:${build_tag}"
            }

        }
        stage('Push Immage') {
            steps {
                echo "3.Push Docker Image Stage"
                withDockerRegistry(credentialsId: 'ecr:cn-north-1:AWS-AKSK', url: 'https://492666533052.dkr.ecr.ap-south-1.amazonaws.com/gitops-app-demo') {
                    sh "docker push ${repo_name}/${app_name}:latest"
                    sh "docker push ${repo_name}/${app_name}:${build_tag}"
                }
            }
        }
    }
}
# test file
