pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'ec2-34-229-84-166.compute-1.amazonaws.com', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'ec2-3-80-56-253.compute-1.amazonaws.com', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('0 * * * *')
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
 
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "echo y | pscp -i \"C:/TCP/initiative/devops/aws/ec2/jenkins-demo2.ppk\" \"C:/Users/rohkhann/Desktop/test.txt\" ec2-user@ec2-34-229-84-166.compute-1.amazonaws.com:/tmp"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "echo y | pscp -i C:/TCP/initiative/devops/aws/ec2/jenkins-demo2.pem C:/Users/rohkhann/Desktop/test.txt ec2-user@ec2-3-80-56-253.compute-1.amazonaws.com:/tmp"
                    }
                }
            }
        }
    }
}