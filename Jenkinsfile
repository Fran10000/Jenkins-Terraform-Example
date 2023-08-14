pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    docker {
            image 'alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
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
    
    stage('tfsec') {
      steps {
            docker.image('aquasec/tfsec').inside('-v /var/jenkins_home/workspace/Tarea_4:/src') {
            sh 'tfsec .'
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
