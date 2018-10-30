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
               git add terraform.tfstate
               git -c user.name="hip911" -c user.email="me@gaborcsikos.xyz" commit -m "terraform state update from Jenkins"
               git push @github.com/hip911/tf-test.git>https://${REPO_USER}:${REPO_PASS}@github.com/hip911/tf-test.git master
            '''
          }
         }
        }
      }
    }
