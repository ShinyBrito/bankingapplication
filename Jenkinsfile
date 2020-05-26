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
                            sshPublisherDesc( configName: configName, verbose: false, transfers: [
                            sshTransfer(
                                sourceFiles: "DemoApp100-1.0.0.tgz ",
                                remoteDirectory: "jenkins-promote",
                                execCommand: "cd /opt
                                                sudo mkdir -p node-bank
                                                sudo pm2 stop index
                                                sudo pm2 delete index
                                                sudo rm -rf  /opt/node-bank/*
                                            sudo mv -v /home/capeuser/demoapp/* /opt/node-bank
                                            cd /opt/node-bank
                                            sudo npm install DemoApp100-1.0.0.tgz 
                                                cd node_modules/DemoApp100
                                                sudo pm2 start  server/index.js  "
                            )
                        ])
                    ])
                }
            }
        }

    }
}
