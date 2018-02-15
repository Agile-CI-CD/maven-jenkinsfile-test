pipeline{    
    
    
    agent any

    parameters {
        string(name: 'tomcat_staging', defaultValue: '54.152.231.43', description: 'staging' )
        string(name: 'tomcat_prod', defaultValue: '54.152.231.43', description: 'prod' )
    }

    
#    #triggers {
 #       pollSCM('* * * * *')
 #   }
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
        
            post {
                success {
                    echo 'Now archiving....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments'){
            parallel{
                stage('Deploy to staging'){
                    steps{
                        sh "scp -i /var/lib/jenkins/Census-Prep.pem -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/FullAutomation1/webapp/target/webapp.war ec2-user@${params.tomcat_staging}:/opt/tomcat-staging/webapps"
                    }
                }

                stage('Deploy to production'){
                    steps{
                        sh "scp -i /var/lib/jenkins/Census-Prep.pem -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/FullAutomation1/webapp/target/webapp.war ec2-user@${params.tomcat_prod}:/opt/tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}
