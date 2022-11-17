pipeline {
	agent none
	stages {
		stage ('OWASP Dependency-Check Vulnerabilities') {
			agent any
			steps {
				dependencyCheck additionalArguments: ''' 
				-o "./" 
				-s "./src"
				-f "ALL" 
				--prettyPrint''', odcInstallation: 'OWASP Dependency Check'
				dependencyCheckPublisher pattern: 'dependency-check-report.xml'
			}
		}
		stage('SonarQube Analysis') {
			agent any
			environment {
				scannerHome = tool 'tokage_sonarqube_scanner'
			}
			steps {
				withSonarQubeEnv('sonarqube') {
					sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=sonarqube-Tokage-webapp -Dsonar.projectName=sonarqube-Tokage-webapp"
				}
			}
			post {
				failure {
					sh 'echo "failure"'
				}
			}
		}
// 		stage('Unit Testing'){
// 			agent{
// 				docker {
// 					image 'composer:latest'
// 				}
// 			}
// 			steps{
//              recordIssues([
//                  enabledForFailure: true,
//                  tools: [php()]
//         ])
// 				sh 'composer install'
// 				sh './vendor/bin/phpunit --log-junit logs/unitreport.xml tests'
// 			}
// 		}
// 		stage('Integration UI Test') {
// 			parallel {
// 				stage('Deploy') {
// 					agent any
// 					steps {
// 						sh 'set -x'
// 						sh 'docker run -d -p 80:80 --name my-apache-php-app -v /var/jenkins_home/workspace/Lab07b_Webapp/src:/var/www/html php:7.2-apache'
// 						sh 'sleep 1'
// 						sh 'set +x'
// 						sh 'echo "Now..."'
// 						sh 'echo "Visit http://localhost to see your PHP application in action."'
// 						input message: 'Finished using the web site? (Click "Proceed" to continue)'
// 						sh 'set -x'
// 						sh 'docker kill my-apache-php-app'
// 						sh 'docker rm my-apache-php-app'
// 					}
// 				}
// 				stage('Headless Browser Test') {
// 					agent {
// 						docker {
// 							image 'maven:3-alpine' 
// 							args '-v /root/.m2:/root/.m2' 
// 						}
// 					}
// 					steps {
// 						sh 'mvn -B -DskipTests clean package'
// 						sh 'mvn test'
// 					}
// 					post {
// 						always {
// 							junit 'target/surefire-reports/*.xml'
// 							junit 'logs/*.xml'
// 						}
// 					}
// 				}
// 			}
//		}
	}
}
