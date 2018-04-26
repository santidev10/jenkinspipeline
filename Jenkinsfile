pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.191.25.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.220.110.18', description: 'Production Server')
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
                        sh "scp -i /var/lib/jenkins/centos7.pem **/target/*.war centos@${params.tomcat_dev}:/usr/share/apache-tomcat-8.5.30/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /var/lib/jenkins/centos7.pem **/target/*.war centos@${params.tomcat_prod}:/usr/share/apache-tomcat-8.5.30/webapps"
                    }
                }
            }
        }
    }
}
