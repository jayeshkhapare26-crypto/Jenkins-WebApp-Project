pipeline {
    agent {label 'Frontend-Node'}
    environment {
        APP_SERVER = "172.31.7.156"
        DEPLOY_DIR =  "/var/www/html"
    }
    stages{
        stage ('pre build actions'){
            steps {
                sh 'npm install'
           }
        }
        stage ('build'){
            steps {
               sh 'npm run build'
            }

        }
        stage ('post build actions'){
            steps {
                sh """
                echo 'cleaning old files on app server'
                ssh jenkins@${APP_SERVER} 'rm -rf ${DEPLOY_DIR}/*'
                echo 'Copying frontend build'
                scp -r dist/* jenkins@${APP_SERVER}:${DEPLOY_DIR}/
                echo 'Restarting nginx'
                ssh jenkins@${APP_SERVER} 'sudo systemctl restart nginx'
                """
            }
        }
    }
}
