pipeline {
  agent any

  options {
    disableConcurrentBuilds()
  }
  
  stages {
    stage('PreReq') {
      steps {
        script {
        sh script: """
                  sudo  yum install python37 -y && sudo yum install python3-pip -y && sudo yum install git -y
                   python3.7 -m pip install --upgrade pip
                   pip3 install botocore 
                   pip3 install boto3 
                   git clone https://github.com/capitalone/cloud-custodian
                   cd cloud-custodian
                   make install
                   source bin/activate
                   cd tools/c7n_org
                   python setup.py develop
                   """
        }
      }
    }
    stage('Validate') {
      steps {
        sh 'custodian validate cloud-c7n-policies/ec2/ebs-volume-delete-unattached.yml'
      }
    }
    stage('dryrun') {
      steps {
        sh 'c7n-org run -c account-config/poc.yml -s output -u  cloud-c7n-policies/ec2/ebs-volume-delete-unattached.yml --dryrun'
      }
    }
    stage('run') {
      steps {
        sh 'c7n-org run -c account-config/poc.yml -s output -u  cloud-c7n-policies/ec2/ebs-volume-delete-unattached.yml'
      }
    }
    // stage('clean-old-functions') {
    //         steps {
    //           sh 'c7n-org run-script -c accounts-test.yml -s .  "python mugc.py -c custodian.yml" '
    //         }
    // }
  }
}
