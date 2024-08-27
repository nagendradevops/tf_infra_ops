pipeline {
    agent any

    environment {
        TF_VAR_ENV = '' // Placeholder for environment variable
    }

    stages {
        stage('Deploy to Dev') {
            steps {
                script {
                    env.TF_VAR_ENV = 'develop'
                    deployTerraform(env.TF_VAR_ENV)
                }
            }
        }
        stage('Approve and Deploy to Int') {
            steps {
                script {
                    input message: 'Approve deployment to Int?'
                    env.TF_VAR_ENV = 'int'
                    deployTerraform(env.TF_VAR_ENV)
                }
            }
        }
        stage('Approve and Deploy to Pre-Prod') {
            steps {
                script {
                    input message: 'Approve deployment to Pre-Prod?'
                    env.TF_VAR_ENV = 'pre-prod'
                    deployTerraform(env.TF_VAR_ENV)
                }
            }
        }
        stage('Approve and Deploy to Prod') {
            steps {
                script {
                    input message: 'Approve deployment to Prod?'
                    env.TF_VAR_ENV = 'prod'
                    deployTerraform(env.TF_VAR_ENV)
                }
            }
        }
    }
}

def deployTerraform(environment) {
    withEnv(["ENV=${environment}"]) {
        sh """
            terraform init -backend-config="key=${environment}/terraform.tfstate"
            terraform plan -var-file=env/${environment}.tfvars -out=tfplan
            terraform apply -auto-approve tfplan
        """
    }
}
