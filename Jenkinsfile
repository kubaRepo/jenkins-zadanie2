pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3"
    }

    stages {
		stage('Dont execute') {
			when {
				not {
					changelog ".*[ci skip].*"
				}
			}
			steps {
				echo 'It shouldnt execute!'
			}
		}
        stage('Clean') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kubaRepo/szkolenie-cicd-jenkins-gitlab-example.git']])
            }
        }
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                // git 'https://github.com/spring-projects/spring-petclinic.git'
                

                // sh 'git checkout main' 
                // Run Maven on a Unix agent.
                sh 'mvn clean verify'
                
                junit '**/target/surefire-reports/TEST-*.xml'
                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
    }
    post { 
        success {
            slackSend channel: '#czajka-jenkins-delete', color: "good", message: "Build success BUILD_TAG: ${env.BUILD_TAG} BUILD_URL: ${env.BUILD_URL}", tokenCredentialId: '9193dfd2-fc4b-4aae-a3ae-211d7b459e38'
        }
        unstable {
            slackSend channel: '#czajka-jenkins-delete', color: "warning", message: 'Build unstable BUILD_TAG: ${env.BUILD_TAG} BUILD_URL: ${env.BUILD_URL}', tokenCredentialId: '9193dfd2-fc4b-4aae-a3ae-211d7b459e38'
        }
        failure {
            slackSend channel: '#czajka-jenkins-delete', color: "danger", message: 'Build failed BUILD_TAG: ${env.BUILD_TAG} BUILD_URL: ${env.BUILD_URL}', tokenCredentialId: '9193dfd2-fc4b-4aae-a3ae-211d7b459e38'
        }
    }
}

