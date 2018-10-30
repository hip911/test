pipeline {
  agent {
    node {
      label 'master'
    }
  } 
    stages {
      stage('AWS Deployment') {
        steps {
          withCredentials([string(credentialsId: 'AWS_KEY', variable: 'AWS_KEY'),string(credentialsId: 'AWS_SECRET', variable: 'AWS_SECRET')]) {
              sh 'rm -rf tf-test'
              sh 'git clone https://github.com/hip911/tf-test.git'
              sh '''
               cd tf-test
               /tmp/terraform init -var access_key=${AWS_KEY} -var secret_key=${AWS_SECRET}
               /tmp/terraform apply -auto-approve -var access_key=${AWS_KEY} -var secret_key=${AWS_SECRET} -var 'key_name=terraformnew' -var 'public_key_path=/tmp/terraform.pub'
            '''
          }
         }
        }
      }
    }
