pipeline {
    agent any
    
    environment {
        AWS_REGION = 'us-east-2'  // Thay đổi vùng AWS của bạn
        EKS_CLUSTER_NAME = 'my-eks-cluster'  // Thay đổi tên cluster EKS của bạn
        DOCKER_IMAGE_NAME = 'quachhungnam/jenkins-k8s-deploy'  // Thay đổi tên Docker image của bạn
        BUILD_NUMBER="1.2"
        // KUBECONFIG = credentials('my-kubeconfig-credentials') // Thay đổi tên credentials cho kubeconfig
    }
    
    stages {
        stage('Init'){
            steps{
                sh 'printenv'
            }
        }

        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                branches: [[name: 'main']],
                userRemoteConfigs: [[url: 'https://github.com/quachhungnam/jk-k8s-deployment.git']]])
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                echo "Build and Push Docker Image"
                // Xây dựng ứng dụng và đóng gói vào Docker image
                script {
                    def customImage =docker.build("${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}")
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-PAT') {
                        customImage.push()
                    }
                }
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                // Triển khai ứng dụng lên EKS bằng kubectl
                echo "Deploy to EKS text"
                // sh """\
                // aws eks --region ${AWS_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}
                // kubectl config use-context ${EKS_CLUSTER_NAME}
                // kubectl set image deployment/my-deployment my-container=${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}
                // """
            }
        }
        
        stage('Cleanup') {
            steps {
                echo "Cleanup"
                // Xoá các tệp và tài nguyên tạm thời (tùy chọn)
            }
        }
    }
    
    post {
        success {
            echo "Success"
            // Thông báo thành công (tùy chọn)
        }
        failure {
            echo "Failure"
            // Thông báo lỗi và thực hiện các xử lý khi thất bại (tùy chọn)
        }
    }
}
