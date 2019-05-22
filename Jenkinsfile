#!/usr/bin/groovy
pipeline {
    environment {
        JAVA_HOME = tool('java')
    }
agent any
options {
disableConcurrentBuilds()
}
        stages {
		    stage ("calculator-munit-mule4_dev"){
			   stages {
			      stage("Build calculator-munit-mule4_dev source code"){
				      steps {
                           slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
                           slackSend (color: "add8e6", message: 'Calculator-munit-mule4_dev Deployment Started')
                           
		                   sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy -Denv=dev'
                           }  
                      }
					 					 
				   stage ('Upload Files To Artifactory'){
				      steps {
					  script{
					     sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
                         def server = Artifactory.server 'artifactory'
                         def uploadSpec = """{
                         "files": [
                           {
                          "pattern": "**/*.jar",
                          "target": "generic-local/calculator-munit-mule4_dev_$BUILD_NUMBER/calculator-munit-mule4.jar"
                            }
                           ]
                         }"""                 
                            def buildInfo1 = server.upload spec: uploadSpec
					  }
					  }
				   }
				   
				  stage('approve'){
				  steps{
				  emailext attachLog: true, mimeType: 'text/html', body: '''The source code is uploaded and is built from DEV group and waiting for your approval to forward to QA Environment:<br> <br>
                    <table border="1">
                    <tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                    <tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                    <tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                    <tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>http://jenkins.eaiesb.com:8891/blue/organizations/jenkins/${JOB_NAME}/detail/${JOB_NAME}/${BUILD_NUMBER}/pipeline</td></tr>
                         </table>''', subject: '$JOB_NAME Job Waiting For Approval', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com'
					  timeout(time: 7, unit: 'DAYS') 
                     {
                        input message: 'Do you want to deploy?', submitter: 'DEVGroup'
                      }		
					}
					}	 
				   }
				   
				   post {
	             failure {
		                  slackSend (color: "0000ff", message: 'Calculator-munit-mule4_dev Build failed')
                          emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
                          <table border="1">
                          <tr><td style="background-color:white;color:red"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                          <tr><td style="background-color:white;color:red"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                          <tr><td style="background-color:white;color:red"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                          <tr><td style="background-color:white;color:red"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
                          </table>
                          ''', subject: 'calculator-munit-mule4_dev Deployment Status', to: 'sudheekarreddy.donapati@eaiesb.com'
                          slackSend (color: "#FF0001",message: 'Calculator-munit-mule4_dev Deployment Failed')
                          }
		         success {
                         slackSend (color: "0000ff", message: 'Calculator-munit-mule4_dev Build sucess')
                         slackSend (color: "#FFA500",message: 'Calculator-munit-mule4_dev Uploaded Sucessfully')
                         emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
                         <table border="1">
                         <tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
                         </table>
                         ''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'sudheekarreddy.donapati@eaiesb.com'    
                        slackSend (color: "#32CD32", message: 'Calculator-munit-mule4_dev Deployment is Sucessful')
                        }
		
		              }
					  }
					 stage("calculator-munit-mule4_qa"){
                     stages{
					    stage("Build calculator-munit-mule4_qa source code"){
				      steps {
                           slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
                           slackSend (color: "add8e6", message: 'Calculator-munit-mule4_qa Deployment Started')
                           
		                   sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy -Denv=qa'
                           }  
                      }	
				   stage ('Upload Files To Artifactory'){
				      steps {
					  script{
					     sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
                         def server = Artifactory.server 'artifactory'
                         def uploadSpec = """{
                         "files": [
                           {
                          "pattern": "**/*.jar",
                          "target": "generic-local/calculator-munit-mule4_qa_$BUILD_NUMBER/calculator-munit-mule4.jar"
                            }
                           ]
                         }"""                 
                            def buildInfo1 = server.upload spec: uploadSpec
					  }
					  }
				   }
				   
					stage('approve'){
					steps{
					emailext attachLog: true, mimeType: 'text/html', body: '''The source code is uploaded and is built from DEV group and waiting for your approval to forward to QA Environment:<br> <br>
                         <table border="1">
                         <tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>http://jenkins.eaiesb.com:8891/blue/organizations/jenkins/${JOB_NAME}/detail/${JOB_NAME}/${BUILD_NUMBER}/pipeline
</td></tr>
                         </table>''', subject: '$JOB_NAME Job Waiting For Approval', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com'					
					  timeout(time: 7, unit: 'DAYS') 
                     {
                        input message: 'Do you want to deploy?', submitter: 'QAGroup'
                      }		
					}
					}	 				   
				   }
				   post {
	             failure {
		                  slackSend (color: "0000ff", message: 'Calculator-munit-mule4_qa Build failed')
                          emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
                          <table border="1">
                          <tr><td style="background-color:white;color:red"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                          <tr><td style="background-color:white;color:red"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                          <tr><td style="background-color:white;color:red"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                          <tr><td style="background-color:white;color:red"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
                          </table>
                          ''', subject: 'calculator-munit-mule4_qa Deployment Status', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com,sudheekarreddy.donapati@eaiesb.com'
                          slackSend (color: "#FF0001",message: 'Calculator-munit-mule4_qa Deployment Failed')
                          }
		         success {
                         slackSend (color: "0000ff", message: 'Calculator-munit-mule4_qa Build sucess')
                         slackSend (color: "#FFA500",message: 'Calculator-munit-mule4_qa Uploaded Sucessfully')
                         emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
                         <table border="1">
                         <tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
                         </table>
                         ''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com,sudheekarreddy.donapati@eaiesb.com'    
                        slackSend (color: "#32CD32", message: 'Calculator-munit-mule4_qa Deployment is Sucessful')
                        }
		
		              }	
					  }
					  stage("calculator-munit-mule4_prod"){
                     stages{
					    stage("Build calculator-munit-mule4_prod source code"){
				      steps {
                           slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
                           slackSend (color: "add8e6", message: 'Calculator-munit-mule4_prod Deployment Started')
                           
		                   sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy -Denv=prod'
                           }  
                      }	
				   stage ('Upload Files To Artifactory'){
				      steps {
					  script{
					     sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
                         def server = Artifactory.server 'artifactory'
                         def uploadSpec = """{
                         "files": [
                           {
                          "pattern": "**/*.jar",
                          "target": "generic-local/calculator-munit-mule4_prod_$BUILD_NUMBER/calculator-munit-mule4.jar"
                            }
                           ]
                         }"""                 
                            def buildInfo1 = server.upload spec: uploadSpec
					  }
					  }
				   }

					stage('approve'){
					steps{
						emailext attachLog: true, mimeType: 'text/html', body: '''The source code is uploaded and is built from QA group and waiting for your approval to forward to PROD Environment:<br> <br>
                         <table border="1">
                         <tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>http://jenkins.eaiesb.com:8891/blue/organizations/jenkins/${JOB_NAME}/detail/${JOB_NAME}/${BUILD_NUMBER}/pipeline
</td></tr>
                         </table>''', subject: '$JOB_NAME Job Waiting For Approval', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com,sudheekarreddy.donapati@eaiesb.com'
					  timeout(time: 7, unit: 'DAYS') 
                     {
                        input message: 'Do you want to deploy?', submitter: 'PRODGroup'
                      }		
					}
					}	 				   
				   }
				   post {
	             failure {
		                  slackSend (color: "0000ff", message: 'Calculator-munit-mule4_prod Build failed')
                          emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
                          <table border="1">
                          <tr><td style="background-color:white;color:red"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                          <tr><td style="background-color:white;color:red"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                          <tr><td style="background-color:white;color:red"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                          <tr><td style="background-color:white;color:red"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
                          </table>
                          ''', subject: 'calculator-munit-mule4_prod Deployment Status', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com,sudheekarreddy.donapati@eaiesb.com'
                          slackSend (color: "#FF0001",message: 'Calculator-munit-mule4_prod Deployment Failed')
                          }
		         success {
                         slackSend (color: "0000ff", message: 'Calculator-munit-mule4_prod Build sucess')
                         slackSend (color: "#FFA500",message: 'Calculator-munit-mule4_prod Uploaded Sucessfully')
                         emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
                         <table border="1">
                         <tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
                         <tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
                         </table>
                         ''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'kiran.padam@eaiesb.com,manoj.gundam@eaiesb.com,sudheekarreddy.donapati@eaiesb.com'    
                        slackSend (color: "#32CD32", message: 'Calculator-munit-mule4_prod Deployment is Sucessful')
                        }
		
		              }				   					  
					}						  
			}
		
}		
