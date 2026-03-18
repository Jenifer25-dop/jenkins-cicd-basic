pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        // Declarative: Checkout SCM runs before this automatically,
        // but we keep a light explicit checkout for clarity.
        git branch: 'main', url: 'https://github.com/Jenifer25-dop/jenkins-cicd-basic.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          echo "Building local image..."
          docker build -t jenkins-simple-cicd:latest .
        '''
      }
    }

    stage('Load Image into Minikube') {
      steps {
        sh '''
          echo "Loading image into Minikube cache..."
          minikube image load jenkins-simple-cicd:latest
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          echo "Applying K8s manifests..."
          kubectl apply -f deployment.yaml
          kubectl apply -f service.yaml

          echo "Waiting for rollout..."
          kubectl rollout status deployment/simple-app --timeout=90s || true

          echo "Current pods:"
          kubectl get pods -o wide
        '''
      }
    }

    stage('Smoke Test (from Jenkins node)') {
      steps {
        sh '''
          echo "If NodePort 30007 is open, you can test from outside using EC2 public IP."
          echo "From this node, quick check via cluster port-forward:"
          kubectl rollout status deployment/simple-app --timeout=30s || true
          kubectl get svc simple-service
        '''
      }
    }
  }
}
