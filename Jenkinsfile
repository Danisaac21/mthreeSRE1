pipeline {
    agent any
    environment {
        WSL_PATH = '/mnt/c/Users/Daniel Long/.jenkins/workspace/Deployment_Pipeline'
        WSL_TMP_PATH = '/mnt/c/tmp'
        ARTIFACT_PATH = 'fake-artifact.zip'
        PLAYBOOK_PATH = '/mnt/c/Users/Daniel Long/.jenkins/workspace/Deployment_Pipeline/deploy-playbook.yml'
        INVENTORY_PATH = '/mnt/c/Users/Daniel Long/.jenkins/workspace/Deployment_Pipeline/inventory'
    }
    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                script {
                    git url: 'https://github.com/Danisaac21/mthreeSRE1.git', branch: 'main', credentialsId: '166b7cc0-41a1-4e8e-93d4-ecb1f00ac5b6'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Simulating artifact creation...'
                bat 'echo "This is a simulated artifact"  1>fake-file.txt'
                bat '"C:\\Program Files\\7-Zip\\7z.exe" a ${ARTIFACT_PATH} fake-file.txt'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archiving build artifact...'
                archiveArtifacts artifacts: "${ARTIFACT_PATH}", allowEmptyArchive: true
            }
        }

        stage('Deploy with Ansible') {
            steps {
                script {
                    echo 'Deploying with Ansible...'
        
                    // Ensure the target directory exists before copying
                    bat """
                        mkdir C:\\wsl\\Ubuntu-20.04\\mnt\\c\\tmp
                    """
        
                    // Copy artifact to the correct WSL path
                    bat """
                        copy ${ARTIFACT_PATH} C:\\wsl\\Ubuntu-20.04\\mnt\\c\\tmp\\${ARTIFACT_PATH}
                    """
        
                    // Run the Ansible playbook from WSL
                    bat """
                        wsl ansible-playbook "${PLAYBOOK_PATH}" -i "${INVENTORY_PATH}"
                    """
                }
            }
        }

        stage('Declarative: Post Actions') {
            steps {
                echo 'Deployment failed. Check logs for details.'
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline was successful!'
        }
        failure {
            echo 'Pipeline failed, please check logs for more details.'
        }
    }
}
