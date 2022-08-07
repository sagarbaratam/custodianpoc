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
                  #sudo  yum install python37 -y && sudo yum install python3-pip -y && sudo yum install git -y
                   #python3.7 -m pip install --upgrade pip
                   #pip3 install botocore 
                   #pip3 install boto3 
                   #git clone https://github.com/capitalone/cloud-custodian
                   cd cloud-custodian
                   #make install
                   #cd tools/c7n_org
                   #/home/ec2-user/venvs/my_venv/bin/python3 setup.py develop
                   #source my_venv activate
                   python3 --version
                    custodian -h
                   """
        }
      }
    }
    
    stage('venv creation'){
      steps{
        sh"""
         python3 -m venv custodian
         source custodian/bin/activate
         pip install c7n
         """
      }
    }
    stage('Validate') {
      steps {
        sh """
        custodian validate cloud-c7n-policies/ec2/ebs-volume-delete-unattached.yml
        """
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
