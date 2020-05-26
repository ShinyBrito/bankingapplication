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
       stage('Deploy'){
            steps{
                script {
                  
                    sshPublisher(
                        continueOnError: false, failOnError: true,
                        publishers: [
                            sshPublisherDesc( configName: "cape-demo-registry.southeastasia.cloudapp.azure.com", verbose: false, transfers: [
                            sshTransfer(
                                sourceFiles: "DemoApp100-1.0.0.tgz ",
                                remoteDirectory: "demoapp",
                                execCommand: "cd /opt \n
                                                sudo mkdir -p node-bank \n
                                                sudo pm2 stop index \n
                                                sudo pm2 delete index \n
                                                sudo rm -rf  /opt/node-bank/* \n
                                            sudo mv -v /home/capeuser/demoapp/* /opt/node-bank \n
                                            cd /opt/node-bank \n
                                            sudo npm install DemoApp100-1.0.0.tgz \n
                                                cd node_modules/DemoApp100 \n
                                                sudo pm2 start  server/index.js  "
                            )
                        ])
                    ])
                }
            }
        }

    }
}
