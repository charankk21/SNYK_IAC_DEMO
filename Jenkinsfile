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
        options { skipDefaultCheckout() }      
        steps {
            script {
                  unstash "SNYK_IAC_DEMO"
                    sh "ls -al"
                    dir('SNYK_IAC_DEMO'){
                    sh "ls -al"
                    sh "pipenv install"
                    sh "pipenv run pip install checkov"
                    sh "pipenv run checkov --directory terraform/ --quiet --download-external-modules true --framework terraform -o junitxml > Checkov.json || true"
                    def report_name='Checkov.json'
                    stash allowEmpty: true, includes: report_name, name: report_name 
                   }
                 }  
              }
          }
stage('defect-dojo')
{
	agent any
	steps
	{
		script
		{
		catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE')  {
			def report_name='Checkov.json'
			//stash allowEmpty: true, includes: report_name, name: 'Checkov.json'
			ws('C:\\Users\\Administrator\\defectdojo_api\\examples\\v2')
			{
				unstash report_name
				bat 'python dojo_ci_cd.py --product=2 --file '+report_name+' --scanner="Checkov Scan" --high=0 --host=https://demo.defectdojo.org --api_token=548afd6fab3bea9794a41b31da0e9404f733e222 --user=admin --engagement=1 --active TRUE'
			}
			bat 'exit 0'
			}
		}
	}
}
	}
}