pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging environment'){
            steps{
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to production environment'){
            steps{
                timeout(time:5, unit:'Days'){
                    input message:'Approve production environment?'
                }
                build job: 'deploy-to-production'
            }
            post{
                success{
                    echo 'Code deployed to production'
                }
                failure{
                    echo ' Deployment Failed'
                }
            }
        }
    }
}
