pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp1975 -Dsonar.organization=asgbuggywebapp1975 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=bf827dbeda9716915d4cce3ae33505ba4e2c1f74'
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
                    docker.withRegistry('https://056613665414.dkr.ecr.ap-southeast-1.amazonaws.com/asg', 'ecr:ap-southeast-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
  }
}
