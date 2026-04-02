pipeline {
        agent any 
        environment {
            IMAGE_NAME   = "taskr"
            IMAGE_TAG    = "${BUILD_NUMBER}"
            REGISTRY     = "urastogi74"            // ← change this
            FULL_IMAGE   = "${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
    }
    
        stages {
            stage('Build') {
                steps {
                    sh "docker build -t ${FULL_IMAGE} ."
                    sh "docker tag ${FULL_IMAGE} ${REGISTRY}/${IMAGE_NAME}:latest"
                    echo "Built: ${FULL_IMAGE}"
            } }
    
            stage('Push') {
                steps {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hu',       // ← Jenkins credential ID
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                            )]) {
                        sh '''
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push ${FULL_IMAGE}
                            docker push ${REGISTRY}/${IMAGE_NAME}:latest
                            '''
                }
            } }
                post {
                    always { sh 'docker logout' }
                        }
        
            stage('Deploy') {
                steps {
                    sh '''
                
                        minikube status
                        kubectl apply -f k8s/pvc.yaml
                        kubectl apply -f k8s/service.yaml
 
                        kubectl apply -f k8s/deployment.yaml
                        kubectl set image deployment/taskr taskr=${FULL_IMAGE}
                        '''
            }
        
     }
    }
}

