pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_dev', defaultvalue: '34.238.120.148', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultvalue: '3.83.174.100', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')

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
                        bat "scp -i C:/Users/abhbhatt/newAWSkey.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps/"
                    }
                }
                stage ('Checkstyle Analysis'){
                    steps {
                        bat 'mvn checkstyle:checkstyle'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i C:/Users/abhbhatt/newAWSkey.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps/"
                    }
                }
            }
        }
    }
}