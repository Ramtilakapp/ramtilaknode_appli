pipeline {
    agent any

    environment {
        DEPLOY_SSH_SITE = 'node_pro_environ'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git url: 'https://github.com/Ramtilakapp/ramtilaknode_appli.git', branch: 'main'
            }
        }
        
       stage('Install Dependencies') {
           steps {
                 Install Node.js dependencies
                sh 'npm install'
            }
    } 
        
       /* stage('Run Tests') {
            steps {
                // Run tests (if any)
                sh 'npm test'
            }
        }*/
        
        stage('Build') {
            steps {
                // Build the application (if applicable)
                sh 'npm run build'
            }
        }
        
        stage('Deploy') {
            steps {
                // Deploy to the Node.js EC2 instance via SSH
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: DEPLOY_SSH_SITE,
                        transfers: [
                            sshTransfer(
                                sourceFiles: '**/*',
                                removePrefix: '', // Adjust this if needed
                                remoteDirectory: '/home/abcd/nodedeploy', // Set this to the deployment directory on the EC2 instance
                                execCommand: 'cd /home/abcd/nodedeploy && pm2 restart all' // Adjust paths and commands as needed
                            )
                        ]
                    )
                ])
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
