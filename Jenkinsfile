pipeline {
    agent any
    environment {
        GIT_EMAIL = 'raj@cloudwithraj.com'
        GIT_NAME = 'RajSaha'
        REPO_URL = 'github.com/${GIT_USERNAME}/kubernetesmanifest.git'
    }
    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh """
                                git config user.email ${env.GIT_EMAIL}
                                git config user.name ${env.GIT_NAME}
                                cat deployment.yaml
                                sed -i 's+raj80dockerid/test.*+raj80dockerid/test:${env.DOCKERTAG}+g' deployment.yaml
                                cat deployment.yaml
                                git add .
                                git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'
                                git push https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@${env.REPO_URL} HEAD:main
                            """
                        }
                    }
                }
            }
        }
    }
}
