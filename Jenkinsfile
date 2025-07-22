pipeline {
  agent any

  environment {
    DOCKERHUB_USER = credentials('Yashwanthgowdan')     
    DOCKERHUB_PASS = credentials('************')     
    AWS_REGION = 'ap-northeast-3'
    EKS_CLUSTER_NAME = 'eksctl-yash-e-commerce-cluster/VPC'
  }

  stages {

    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/Yashwanthgowdan/E-CommerceStore.git', branch: 'main'
      }
    }

    stage('Build and Push Docker Images') {
      steps {
        script {
          def services = [
            'user-service',
            'product-service',
            'cart-service',
            'order-service'
          ]

          services.each { service ->
            dir("backend/${service}") {
              sh """
                echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
                docker build -t ${DOCKERHUB_USER}/${service}:latest .
                docker push ${DOCKERHUB_USER}/${service}:latest
              """
            }
          }

          dir("frontend") {
            sh """
              echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USER}" --password-stdin
              docker build -t ${DOCKERHUB_USER}/frontend:latest .
              docker push ${DOCKERHUB_USER}/frontend:latest
            """
          }
        }
      }
    }

    stage('Update Kubeconfig') {
      steps {
        sh "aws eks --region ${AWS_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh "kubectl apply -f k8s/"
      }
    }
  }

  post {
    success {
      echo "✅ Deployment to EKS successful!"
    }
    failure {
      echo "❌ Deployment failed. Check logs."
    }
  }
}
