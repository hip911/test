pipeline {
  agent {
    docker {
      image 'hip911/agent-image:latest'
    }

  }
  stages {
    stage('Create Packer AMI') {
      steps {
        withCredentials(bindings: [
                      usernamePassword(credentialsId: 'awsCred', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_KEY')
                    ]) {
            sh 'packer build -debug -var aws_access_key=${AWS_KEY} -var aws_secret_key=${AWS_SECRET} packer/packer.json'
          }

        }
      }
      stage('AWS Deployment') {
        steps {
          withCredentials(bindings: [
                        usernamePassword(credentialsId: 'awsCred', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_KEY'),
                        usernamePassword(credentialsId: 'repoCred', passwordVariable: 'REPO_PASS', usernameVariable: 'REPO_USER'),
                      ]) {
              sh 'rm -rf repository'
              sh 'git clone https://github.com/hip911/tf-test.git'
              sh '''
               cd repository
               terraform init
               terraform apply -auto-approve -var access_key=${AWS_KEY} -var secret_key=${AWS_SECRET}
               git add terraform.tfstate
               git -c user.name="hip911" -c user.email="me@gaborcsikos.xyz" commit -m "terraform state update from Jenkins"
               git push @github.com/suhasulun/repository.git">https://${REPO_USER}:${REPO_PASS}@github.com/hip911/tf-test.git master
            '''
            }

          }
        }
      }
    }