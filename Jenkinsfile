pipeline{
    agent any
    stages{
       stage('Build') {
            steps{
                sh "rm -rf *.tgz"
                sh "npm install"
                sh "npm pack"
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'SonarScanner'
            }
            steps{
                withSonarQubeEnv('SONAR') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

       stage('Deploy'){
            steps{
               sh "curl -X POST http://cape-win.southeastasia.cloudapp.azure.com:8081/api/packages/raw?replace=true -H 'X-Octopus-ApiKey:API-Z7WXGKHQHUS8WS859ABNUKQ428' -F 'data=@BankAppNodeExample-1.0.0.tgz'"  
            }
        }

    }
}
