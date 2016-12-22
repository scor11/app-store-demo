pipeline {
    agent docker:'maven:3.3.3'
    stages {
        stage ('Build') {
			steps{
				sh 'mvn clean source:jar package'
			}
        }
        stage ('Browser Tests') {
			steps{
				parallel (
					'Firefox': {
						sh "echo 'setting up selenium environment'"
						sh 'sleep 5'
					},
					'Safari': {
						sh "echo 'setting up selenium environment'"
						sh 'sleep 5'
					},
					'Chrome': {
						sh "echo 'setting up selenium environment'"
						sh 'sleep 3'
					},
					'Internet Explorer': {
						sh "echo 'setting up selenium environment'"
						sh 'sleep 3'
					}
				  )
			}
        }
        stage ('Static Analysis') {
			steps{
				sh 'mvn findbugs:findbugs'
			}
        }
        stage ('Package') {
			steps{
				sh 'mvn source:jar package -Dmaven.test.skip'
			}
        }
    }

	
    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
            archive '**/target/*.jar'
        }
    }
	
}