pipeline {
  agent any
  tools { 
        maven 'maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asecurgurubuggywebapp -Dsonar.organization=gitlabdevsecopsci -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=47c40c99a42c0de98747ff17456d27053fd1d4c8'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://386335327471.dkr.ecr.eu-west-1.amazonaws.com/', 'ecr:eu-west-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
