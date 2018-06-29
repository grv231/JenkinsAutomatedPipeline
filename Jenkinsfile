pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '54.153.121.139', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.57.204.205', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving starts here...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging environment'){
                    steps {
                        sh "scp -i /c/Users/grvtr/Desktop/InfluenceHealth_Project/AlternativeFiles/Redis-key.pem **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production environment"){
                    steps {
                        sh "scp -i /c/Users/grvtr/Desktop/InfluenceHealth_Project/AlternativeFiles/Redis-key.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}