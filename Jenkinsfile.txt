pipeline {
    agent any
    stages {
        stage('Retrieve CloudFormation Template') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mykolatereshchenko-lab/cloudformation-lab3.1.git'
            }
        }
        stage('Deploy CloudFormation Stack') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    bat """
                        "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation deploy ^
                            --region us-east-1 ^
                            --stack-name lab3-1 ^
                            --template-file AWSTemplate.yaml ^
                            --capabilities CAPABILITY_IAM ^
                            --no-fail-on-empty-changeset
                    """
                }
            }
        }
    }
}
