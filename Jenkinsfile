pipeline {
    agent  any
    stages {
        stage('build') {
            steps {
                echo  'build done for jira'
            }
        }

        stage('deployments') {
                parallel {
                    stage('deploy to stg') {
                        steps {
                            echo 'stg deployment done'
                        }
                    }
                    stage('deploy to prod') {
                        steps {
                            echo 'prod deployment done'
                        }
                    }
                }
            }    
    }
}
