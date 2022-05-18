pipeline{
    agent any
    stages{
	stage('Checkov ->Checkout'){
        agent any
        steps{
            script{
            deleteDir()
           bat "git clone https://github.com/charankk21/SNYK_IAC_DEMO.git"
           stash "SNYK_IAC_DEMO"
            }
        }
        
    }
    stage('Checkov scan') {
        agent{
           
             docker {
                image 'kennethreitz/pipenv:latest'
                args '-u root --privileged -v /var/run/docker.sock:/var/run/docker.sock'
                label 'linagent'
                }
              
        }
       steps {
            script {

                   unstash "SNYK_IAC_DEMO"
                    sh "ls -al"
                    dir('SNYK_IAC_DEMO'){
                    sh "ls -al"
                    sh "pipenv install"
                    sh "pipenv run pip install checkov"
                    sh "pipenv run checkov --directory terraform/ --quiet --download-external-modules true --framework terraform -o junitxml > result.xml || true"
                    junit allowEmptyResults: true, skipMarkingBuildUnstable: true, skipPublishingChecks: true, testResults: '*.xml' }
                 }  
              }
          }
	}
}