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
                   python3 -m venv custodian
                   source custodian/bin/activate
                   pip install c7n
                   pip install c7n-org

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