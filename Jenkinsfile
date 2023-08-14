pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    tools
    {
        'org.jenkinsci.plugins.docker.commons.tools.DockerTool' 'Docker'
        'terraform' 'Terraform'
    }
    // environment {
    // DOCKER_CERT_PATH = credentials('tarea4')
    // }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('checkout') {
            steps {
                checkout scm
            }
        }

    docker {
            image 'alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    stage('tfsec') {
      steps {
        sh '''docker --version
              docker run --rm -v "/var/jenkins_home/workspace/Tarea_4:/src" aquasec/tfsec . '''
      }
    }
    stage('Approval for Terraform') {
            steps {
                input(message: 'Approval required before Terraform', ok: 'Proceed', submitterParameter: 'APPROVER')
            }
        }

        stage('terraform') {
            steps {
              sh 'terraform init'
              sh 'terraform apply -auto-approve -no-color'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
