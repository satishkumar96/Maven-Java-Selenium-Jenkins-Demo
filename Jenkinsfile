pipeline {
    agent any
    
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MAVEN_HOME"
    }

    stages 
  {
        stage('Build') 
      {
            steps 
        {
                cleanWs()
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/satishkumar96/Maven-Java-Selenium-Jenkins-Demo.git'
            }
        }
    
        stage('Test')
        {

            steps{
                // To run Maven on a Windows agent, use
                bat 'mvn clean verify'
            }

            post {
                
                always 
                {
                	publishHTML([
                	allowMissing: false, 
                	alwaysLinkToLastBuild: false, 
                	keepAll: false, 
                	reportDir: 'target/surefire-reports', 
                	reportFiles: 'emailable-report.html', 
                	reportName: 'HTML_Report', 
                	reportTitles: ''])
                }
                
                failure
                {
                      emailext attachLog: true, 
                      attachmentsPattern: 'target/surefire-reports/emailable-report.html', 
                      body: '''Hello Everybody,

                    The execution of PSC Automation Testing in Dev environment has failed. We are looking into the issue and would re-run the automation job upon rectifying the issue.

                    Regards,
                    QA Team''', 
                      
                      subject: '[$BUILD_STATUS] - $PROJECT_NAME - Build # $BUILD_NUMBER ($BUILD_ID)', 
                      to: 'automationwithsatish@gmail.com'
                }
                
                success
                {
                    emailext attachLog: true, 
                      attachmentsPattern: 'target/surefire-reports/emailable-report.html', 
                      body: '''The automated test execution of PSC Smoke Test Cases is completed. Please find the test report in the below FTP folder, PSC Automation Testing Report - Beta.
                    
                    	Regards,
                    	QA Team''', 
                      subject: '[$BUILD_STATUS] - $PROJECT_NAME - Build # $BUILD_NUMBER ($BUILD_ID)', 
                      to: 'automationwithsatish@gmail.com'
                }
            }
        }
    }
}
