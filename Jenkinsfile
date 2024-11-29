pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Danisaac21/mthreeSRE1.git'
        ARTIFACT_NAME = 'fake-artifact'
        PLAYBOOK_FILE = 'deploy-playbook.yml'
        INVENTORY_FILE = 'localhost'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                git branch: 'main', 
                    credentialsId: '166b7cc0-41a1-4e8e-93d4-ecb1f00ac5b6', 
                    url: GIT_REPO
            }
        }

        stage('Build') {
            steps {
                echo 'Simulating artifact creation...'
                bat '''
                    echo "This is a simulated artifact" > fake-file.txt
                    "C:/Program Files/7-Zip/7z.exe" a fake-artifact.zip fake-file.txt
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archiving build artifact...'
                archiveArtifacts artifacts: "**/target/${ARTIFACT_NAME}.zip", allowEmptyArchive: false
            }
        }

        stage('Deploy with Ansible') {
            steps {
                echo 'Deploying to target servers with Ansible...'
                ansiblePlaybook(
                    playbook: PLAYBOOK_FILE,
                    inventory: INVENTORY_FILE
                )
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check logs for details.'
        }
    }
}
