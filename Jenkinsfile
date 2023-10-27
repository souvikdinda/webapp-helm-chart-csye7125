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
                    sh 'npm ci'
                }
            }
        }

        stage('Semantic-Release') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github_token', usernameVariable: 'GH_USERNAME', passwordVariable: 'GH_TOKEN')]) {
                        env.GIT_LOCAL_BRANCH = 'main'
                        def newVersion = sh(script: 'npx semantic-release --dry-run', returnStdout: true).trim()
                        if (newVersion) {
                            echo "New version: ${newVersion}"
                            def latestCommitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                            echo "Latest commit message: ${latestCommitMessage}"
                            sh "sed -i '0,/version: .*/s/version: .*/version: ${newVersion}/' Chart.yaml"
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