pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.154.160.232', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.252.65.48', description: 'Production Server')
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
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                       sh "scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -i /home/einfochips/Desktop/Key/MasterKeyChetan.pem **/target/*.war ec2-user@${params.tomcat_dev}:/home/ec2-user/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -i /home/einfochips/Desktop/Key/MasterKeyChetan.pem **/target/*.war ec2-user@${params.tomcat_prod}:/home/ec2-user/"
                    }
                }
            }
        }
    }
}