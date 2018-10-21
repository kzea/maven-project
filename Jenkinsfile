pipeline {
    agent any

    tools {
        maven 'localMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.222.164.40', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.222.164.40', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "scp -i /Users/kzea/Documents/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@118.222.164.40:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/kzea/Documents/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@118.222.164.40:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
