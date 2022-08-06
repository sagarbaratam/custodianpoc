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
                   yum install python37 -y
                   yum install python-pip -y
                   yum install git -y
                   pip install botocore –upgrade
                   pip install boto3 –upgrade
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
        sh 'custodian validate cloud-c7n-policies/ec2/aebs-volume-delete-unattached.yml'
      }
    }
    stage('dryrun') {
            steps {
              sh 'c7n-org run -c account-config/poc.yml -s output -u  cloud-c7n-policies/ec2/aebs-volume-delete-unattached.yml --dryrun'
            }
    }
    stage('run') {
            steps {
              sh 'c7n-org run -c accounts-test.yml -s output -u  cloud-c7n-policies/ec2/aebs-volume-delete-unattached.yml'
            }
    }
    // stage('clean-old-functions') {
    //         steps {
    //           sh 'c7n-org run-script -c accounts-test.yml -s .  "python mugc.py -c custodian.yml" '
    //         }
    // }
  }
}