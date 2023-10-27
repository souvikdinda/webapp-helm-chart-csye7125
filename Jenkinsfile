pipeline {
    agent any

    stages{
        stage('Checkout'){
            steps{
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']],
                        userRemoteConfigs: [[credentialsId: 'github_token', url: 'https://github.com/csye7125-fall2023-group07/webapp-helm-chart.git']]
                    ])
                }
            }
        }

        stage('Install Dependencies') {
            steps{
                script {
                    sh "sudo npm install -g semantic-release @semantic-release/git @semantic-release/github"
                }
            }
        }

        stage('Semantic-Release') {
            steps {
                script {
                    def newVersion = sh(script: 'semantic-release --dry-run', returnStdout: true).trim()
                    if (newVersion) {
                        echo "New version: ${newVersion}"
                        def latestCommitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                        echo "Latest commit message: ${latestCommitMessage}"
                        sh "sed -i 's/version: .*/version: ${newVersion}/' Chart.yaml"
                        sh 'helm package .'
                        sh "github-release create -t ${newVersion} -n '${newVersion}' -d '${latestCommitMessage}' *.tgz"
                    } else {
                        error "Failed to capture the new version from semantic-release."
                    }
                }
            }
        }
    }
}