pipeline {
  agent {
    node {
      label 'master'
    }
  }
  stages {
      stage('AWS Deployment') {
        steps {
              sh 'rm -rf repository'
              sh 'git clone https://github.com/hip911/tf-test.git'
              sh '''
               cd repository
               terraform init
               terraform apply -auto-approve -var access_key=${AWS_KEY} -var secret_key=${AWS_SECRET}
               git add terraform.tfstate
               git -c user.name="hip911" -c user.email="me@gaborcsikos.xyz" commit -m "terraform state update from Jenkins"
               git push @github.com/hip911/tf-test.git">https://${REPO_USER}:${REPO_PASS}@github.com/hip911/tf-test.git master
            '''
          }
        }
      }
    }
