pipeline {
    agent {
		node{ label 'kubslave'} 
		}
    
     environment {
     //   JOB_CAUSES = edtUtil.getCauses()
        SONARQUBE_API_KEY = credentials('SonarQube-Token-Latest')
        APPLICATION_VERSION = "${params.version}"

       }				
   	stages {
       /* stage('SCM') {
            steps {
             deleteDir()
					git credentialsId: 'NewtGithub', url: 'https://github.com/newttesting/vul-broken-app.git'
                     script {
							pom = readMavenPom file: 'pom.xml'
                            APPLICATION_VERSION = pom.version.toString()
                      						
							}
            }
        }*/
					
            stage('Build') {
            steps {
                echo "*****Build*****"
                sh "mvn clean install" 
            }
         
		   post {
                success { 
                    junit "**/target/surefire-reports/*.xml"
                }
            }
		}
		stage ('Start Application') {
            steps {
                  sh """
                  
                  sudo systemctl start vulapp
                  """
            }
		} 
		stage ('Start ZAP test') {
            steps {
                 build 'ZAP_CI_Demo'
            }
		} 
		stage ('Stop Application') {
            steps {
                  sh """
                  
                  sudo systemctl stop vulapp
                  """
            }
		} 
	}
}
