pipeline {
    agent  any
    stages {
        // stage('build') {
        //     steps {
        //         echo  'build done for jira'
        //     }
        // }
        stage('build') {
                steps {
                    sh '''
                      cd WebApplication1/WebApplication1
                      dotnet build
                      sudo rm -rf ./output
                      sudo mkdir ./output
                      sudo chmod 777 ./output
                      dotnet publish -o ./output
                       
                    '''
                }
        }
                       
                       
                 
    
        stage('stop iis') {
                steps {
                    
                    sh '''
                      sudo sshpass -p ZojItabtsGNCo003gluut*8vz1Vc6hQx ssh -i "mumbai-key.pem" -o StrictHostKeyChecking=no administrator@35.154.6.18 iisreset /stop
                      
                    '''   
                }
        }
    
        stage('deploy to prod') {
                steps {
                    
                    sh '''
                      cd WebApplication1/WebApplication1/
    
                      sudo sshpass -p ZojItabtsGNCo003gluut*8vz1Vc6hQx scp -i "/var/lib/jenkins/workspace/dotnet/mumbai-key.pem" -o StrictHostKeyChecking=no -r ./output/* administrator@35.154.6.18:c:/my-publish
    
                    '''   
                }
        }
        // stage('deployments') {
        //         parallel {
        //             stage('deploy to stg') {
        //                 steps {
        //                     echo 'stg deployment done'
        //                 }
        //             }
        //             stage('deploy to prod') {
        //                 steps {
        //                     echo 'prod deployment done'
        //                 }
        //             }
        //         }
        //     }    
    }
}
