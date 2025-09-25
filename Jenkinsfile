pipeline {
    agent { label 'vkagent' }

    environment {
        GIT_REPO = 'https://github.com/vishnuadur/pipeline-demo1.git'
        BRANCH = 'main'
    }
    
    stages {
        stage('Clean Workspace') {
            steps {
                echo 'CLeaning workspace'
                deleteDir()
            }
        }
        stage('Lint') {
            steps {
                echo "Cloning the repo from Gitlab ........."
                git branch: "${BRANCH}",
                    url: "${GIT_REPO}", 
                    credentialsId: '3d5896a1-d0ab-4b32-8e30-303784e47d51'
            }
        }
        stage('Build') {
            steps {
                sh 'dos2unix build.sh'
                sh 'chmod +x build.sh'
                sh 'bash build.sh'
            }
        }
    }
}
// The `post` block must be a top-level section, at the same level as the `pipeline` block.
post {
    always {
        // This is an optional block.
    }
    unstable {
        echo 'Build marked as UNSTABLE!'
        emailext (
            subject: "Build Unstable: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
            body: """<p>Build became <b>UNSTABLE</b> in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>""",
            recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        )
    }
    failure {
        echo 'Build failed!'
        emailext (
            subject: "Build Failed: ${env.JOB_NAME} [#${env.BUILD_NUMBER}]",
            body: """<p>Build failed in job <b>${env.JOB_NAME}</b> [#${env.BUILD_NUMBER}]</p>""",
            recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        )
    }
}
