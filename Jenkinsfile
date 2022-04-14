node{
    withCredentials([string(credentialsId: 'CK_snykKey', variable: 'snyktoken')]) {
        stage("Snyk-IAC Scan"){
      
           checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/charankk21/SNYK_IAC_DEMO.git']]])
           bat 'C:\\Users\\Administrator\\Downloads\\snyk-win.exe auth ' +snyktoken 
           bat 'C:\\Users\\Administrator\\Downloads\\snyk-win.exe iac test'
           
        }
    }
}
