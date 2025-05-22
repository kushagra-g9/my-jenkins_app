pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kushagrag99/my-app"
        REGISTRY_CREDENTIALS = 'docker-cred'       // Jenkins credential ID for DockerHub
        KUBECONFIG_CREDENTIAL_ID = 'kubeconfig-jenkins' // Jenkins credential ID for kubeconfig
        PATH = "${env.PATH}:/usr/local/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/kushagra-g9/my-jenkins_app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    IMAGE_TAG = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    env.IMAGE_TAG = IMAGE_TAG
                    sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to DockerHub') {
    steps {
        script {
           
            echo "Pushing image: ${DOCKER_IMAGE}:${IMAGE_TAG}"
        }
        withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh """
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                docker push ${DOCKER_IMAGE}:${IMAGE_TAG}
            """
        }
    }
}
        stage('Check kubectl') {
          steps {
               sh 'which kubectl && kubectl version --client'
    }
}


        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIAL_ID}", variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG_FILE
                        sed "s|<IMAGE>|${DOCKER_IMAGE}:${IMAGE_TAG}|g" deployment-and-service.yml | kubectl apply -f -
                    '''
                }
            }
        }
    

    stage('Verify Deployment') {
    steps {
        withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIAL_ID}", variable: 'KUBECONFIG_FILE')]) {
            sh '''
                export KUBECONFIG=$KUBECONFIG_FILE
                kubectl rollout status deployment/my-node-app
                kubectl get svc
            '''
        }
    }
}
    }


    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
        always {
            cleanWs()
        }
    }
}
