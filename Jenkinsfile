pipeline {
  agent any
  environment {
    APPSYSID = 'cb55396287d301102cb687780cbb35c3'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALSDEV = '9dd26136-153a-40b9-b053-ed751fe1e885'
    CREDENTIALSTEST = 'dac1cde3-fa06-4eff-8232-fa6caeccf494'
    CREDENTIALSPROD = '2121c490-8ceb-4160-aad4-e2dbe3cff681'
    DEVENV = 'https://emprstep1dev.service-now.com/'
    TESTENV = 'https://emprstep1test.service-now.com/'
    PRODENV = 'https://emprstep1prod.service-now.com/'
    TESTSUITEID = 'ab72ec6b8f033300a8616c7827bdee01'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALSDEV}")
        snPublishApp(credentialsId: "${CREDENTIALSDEV}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALSTEST}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALSTEST}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALSPROD}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
