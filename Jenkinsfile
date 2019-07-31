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
					
            stage('SonarQube') {
            steps {
                echo "*****Build*****"
                sh "mvn clean install -DskipTests=false"
				sh "mvn org.owasp:dependency-check-maven:check"
                sh "mvn test verify surefire-report:report-only sonar:sonar -Dsonar.host.url=http://192.168.2.78:9000 -Dsonar.login=5516211574596dc9fa09edd2d41cc4fb001aab70 -Dsonar.password="
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
