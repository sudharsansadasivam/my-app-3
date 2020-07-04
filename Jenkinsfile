pipeline{
   agent any
   tools {
        maven "3.6.3" // You need to add a maven with name "3.6.0" in the Global Tools Configuration page
    }
   stages{
   stage('SCM Checkout'){
      
      steps{
            echo "SCM Checkout Started"
	        script{
		            git 'https://github.com/sudharsansadasivam/my-app-2'
	            }
            echo "SCM Checkout Completed"
      }
   }
   stage('Compile-Package'){
      steps{
         script{
	 sh "mvn -version"
         sh "mvn clean install"
         sh "mvn package"
	 echo "Compile Stage Passed"
	 }
   }
   }
   
   stage('SonarQube Analysis') {
      steps{
         script{
         
        withSonarQubeEnv('sonar') { 
          sh "mvn sonar:sonar"
	}
        }
        }
    }
     
  stage("Quality Gate Statuc Check"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                   slackSend baseUrl: 'https://hooks.slack.com/services/',
                   channel: '#jenkins-pipeline-demo',
                   color: 'danger', 
                   message: 'SonarQube Analysis Failed', 
                   teamDomain: 'DevopsDomain',
                   tokenCredentialId: 'slack-demo'
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }    
   
  /* 
   stage('Email Notification'){
      mail bcc: '', body: '''Hi Welcome to jenkins email alerts
      Thanks
      Sudharsan''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'sudharsansadasivam@gmail.com'
   }
   */
   
   stage('Slack Notification'){
       slackSend baseUrl: 'https://hooks.slack.com/services/',
       channel: '#jenkins-pipeline',
       color: 'good', 
       message: 'Welcome to Jenkins, Slack!', 
       teamDomain: 'Devops',
       tokenCredentialId: 'slack-ID'
   
}
}
