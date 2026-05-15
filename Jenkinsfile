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
                    string(credentialsId: 'aws_access_key_id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws_secret_access_key', variable: 'AWS_SECRET_ACCESS_KEY'),
                    string(credentialsId: 'aws_session_token', variable: 'AWS_SESSION_TOKEN')
                ]) {
                    bat """
                        set AWS_ACCESS_KEY_ID=%AWS_ACCESS_KEY_ID%
                        set AWS_SECRET_ACCESS_KEY=%AWS_SECRET_ACCESS_KEY%
                        set AWS_SESSION_TOKEN=%AWS_SESSION_TOKEN%
                        set AWS_DEFAULT_REGION=us-east-1
                        set AWS_CONFIG_FILE=NUL
                        set AWS_SHARED_CREDENTIALS_FILE=NUL
                        "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation deploy ^
                            --region us-east-1 ^
                            --stack-name wordpress-stack ^
                            --template-file AWSTemplate.yaml ^
                            --capabilities CAPABILITY_IAM ^
                            --parameter-overrides KeyName=vockey ^
                            --no-fail-on-empty-changeset
                    """
                }
            }
        }
        stage('Get WordPress URL') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws_access_key_id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws_secret_access_key', variable: 'AWS_SECRET_ACCESS_KEY'),
                    string(credentialsId: 'aws_session_token', variable: 'AWS_SESSION_TOKEN')
                ]) {
                    bat """
    set AWS_ACCESS_KEY_ID=%AWS_ACCESS_KEY_ID%
    set AWS_SECRET_ACCESS_KEY=%AWS_SECRET_ACCESS_KEY%
    set AWS_SESSION_TOKEN=%AWS_SESSION_TOKEN%
    set AWS_DEFAULT_REGION=us-east-1
    set AWS_CONFIG_FILE=NUL
    set AWS_SHARED_CREDENTIALS_FILE=NUL
    "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" cloudformation deploy ^
        --region us-east-1 ^
        --stack-name wordpress-stack ^
        --template-file AWSTemplate.yaml ^
        --capabilities CAPABILITY_IAM ^
        --parameter-overrides KeyName=vockey ^
        --no-fail-on-empty-changeset
"""
                }
            }
        }
    }
}
