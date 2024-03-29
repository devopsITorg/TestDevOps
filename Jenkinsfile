pipeline {
    agent any
     environment {
        // Define your secret token
        APPROVAL_SECRET_TOKEN = 'MikeTest'
        // Define an environment variable to track approval status
        APPROVAL_STATUS = 'pending'
    }
    stages{
        stage('Checkout') {
        steps {
        script {
            //Define Git credentials (if not already configured in Jenkins)
            git url: 'https://github.com/g34conflux/LionKing.git'
        }
        }
      }
       
        stage('Build') {
            steps {
                // script {
                    // Change the directory to the 'hakunamatata' directory
               bat 'echo "building"'

                    // Build the project using msbuild
                 //   bat 'msbuild /t:Package hakunamatata/hakunamatata.csproj'

                    // cd hakunamatata
                //dotnet build
                // sudo rm -rf ./output
                //  sudo mkdir ./output
                //   sudo chmod 777 ./output
                //   dotnet publish -o ./output
                // }
            }
            
        }
        stage('Send Approval Email') {
            steps {
                script {
                   
                    def emailContent = '''
                        <p>Please review and approve this build:</p>
                        <p>Build URL: ${BUILD_URL}</p>
                        <p>Build Number: ${currentBuild.number}</p>
                        <p>Project: ${JOB_NAME}</p>
                        <p>Build Status: ${currentBuild.currentResult}</p>
                        <p>Click <a href="localhost:8888/job/MyPipeline1/$currentBuild.number?token=${env.APPROVAL_SECRET_TOKEN}">here</a> to approve the build.</p>
                    '''

                    emailext (
                      subject: "Waiting for your Approval! Job: '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                      body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                                  <p>Check console output at ;<a href='${env.BUILD_URL}input'></a>;</p>""",
                        to: 'jenkinconfig@gmail.com'
              )
              
             env.approvalstatus = input message: 'You want to approve this build? ', ok: 'Submit', parameters: [choice(choices: ['Approved', 'Rejected'], name: 'ApprovalStatus')]
                
                echo env.approvalstatus
                }
            }
        }

        stage('Deploy') {
            when {
                expression { env.approvalstatus == "Approved"}
            }
            steps {
                script {
                    echo 'Build approved. Proceeding with deployment.'
                   bat 'msdeploy -verb:sync -source:contentPath="C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\MyPipelineForAWS\\hakunamatata" -dest:contentPath="C:\\DevWebSite"'
                    // Add your deployment steps here
                 //   sh '''
                 // cd hakunamatata

               //   sudo sshpass -p ZojItabtsGNCo003gluut*8vz1Vc6hQx scp -i "C:/mumbai-key.pem" -o StrictHostKeyChecking=no -r ./output/* administrator@35.154.6.18:c:/my-publish

              //  '''   
                 //sudo sshpass -p ZojItabtsGNCo003gluut8vz1Vc6hQx scp -i "/home/ubuntu/mumbai-key.pem" -o StrictHostKeyChecking=no -r ./output/ administrator@35.154.6.18:c:/my-publish
                }
            }
            post {
                always {
                    jiraSendDeploymentInfo site: 'prissoft.atlassian.net', environmentId: 'prod-1', environmentName: 'Production', environmentType: 'production'
                    
                }
            }
        }
    
}
        
}
