pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.191.25.154', description: 'Staging Server')   
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
        stage ('Deploy to Staging'){
            steps {
                sh "scp -i /var/lib/jenkins/centos7.pem **/target/*.war centos@${params.tomcat_dev}:/usr/share/apache-tomcat-8.5.30/webapps"
            }
        }
    }
}
