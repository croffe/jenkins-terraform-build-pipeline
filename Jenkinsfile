pipeline {
    agent any

    stages {
        stage('Validate') {
            steps {
                echo 'Validating..'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                withCredentials([azureServicePrincipal('azure_cr_ft_sp')]) {
                    sh '''
                    mkdir temp
                    cd temp
                    git clone https://github.com/croffe/tf-templates
                    cd tf-templates/${deploymentPlatform}/${deploymentBlueprint}
                    wget -nv https://releases.hashicorp.com/terraform/0.11.10/terraform_0.11.10_linux_amd64.zip
                    unzip terraform_0.11.10_linux_amd64.zip
                    export ARM_CLIENT_ID=$AZURE_CLIENT_ID
                    export ARM_CLIENT_SECRET=$AZURE_CLIENT_SECRET
                    export ARM_SUBSCRIPTION_ID=$AZURE_SUBSCRIPTION_ID
                    export ARM_TENANT_ID=$AZURE_TENANT_ID
                    ./terraform init
                    ./terraform apply -auto-approve -var "name_prefix=${deploymentName}" -var "size=${deploymentSize}" -var "region=${deploymentRegion}"
                    '''
               }
            }
        }
        stage('Report') {
            steps {
                echo 'Reporting....'
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
