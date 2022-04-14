node{
    withCredentials([string(credentialsId: 'CK_snykKey', variable: 'snyktoken')]) {
        stage("Snyk-IAC Scan"){
      
           git 'https://github.com/charankk21/terragoat.git'
           bat 'C:\\Users\\Administrator\\Downloads\\snyk-win.exe auth ' +snyktoken 
           bat 'C:\\Users\\Administrator\\Downloads\\snyk-win.exe iac test'
           
        }
    }
}