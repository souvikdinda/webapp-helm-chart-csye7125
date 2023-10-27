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
                    sh 'npm config set prefix /tmp/npm-global'
                    sh "npm install -g semantic-release @semantic-release/git @semantic-release/github --prefix /tmp/npm-global"
                }
            }
        }

        stage('Semantic-Release') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github_token', usernameVariable: 'GH_USERNAME', passwordVariable: 'GH_TOKEN')]) {
                        withEnv(["GH_TOKEN=${GH_TOKEN}"]) {
                            def newVersion = sh(script: '/tmp/npm-global/bin/semantic-release --dry-run', returnStdout: true).trim()
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
    }
}