pipeline {
    agent any
    environment{
        FUNCTION_NAME="s3toDynamonCSVImport"
        BUCKETS3="javier-csv-loader-bucket"
        CSV="data.csv"
        CODE="lambda_function.py"
        BRANCH_NAME="master"
    }

    stages {
        stage('INIT') {
            steps {
                echo "Initializing Pipeline"
                sh 'aws sts get-caller-identity'
            }
        }
        stage('AWS S3 ls') {
            steps {
                sh 'aws s3 ls'
            }
        } 
        stage('Git Clone') {
            steps {
                sh 'rm -rf s3toDynamonCSVImport/'
                sh 'git clone https://github.com/javiergiuga/s3toDynamonCSVImport.git'
                sh 'ls -lrt s3toDynamonCSVImport/'
            }
        }  
        stage('Upload to S3') {
            steps {
                sh 'aws s3 cp $CSV s3://${BUCKETS3}'
            }
        } 
        stage('Deploy to Lambda') {
            steps {
                sh 'aws lambda update-function-code --function-name $FUNCTION_NAME --s3-bucket ${BUCKETS3} --s3-key data.csv --publish'
            }
        }   
    }
}
