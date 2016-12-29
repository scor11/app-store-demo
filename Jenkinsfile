pipeline {
    agent docker:'maven:3.3.3'
    stages {
        stage ('Build') {
            steps{
               configFileProvider(
                    [configFile(fileId: '327fa783-20f9-453c-8e5d-6404ee8c8ba8', variable: 'MAVEN_SETTINGS')]) {
                        sh 'mvn -s $MAVEN_SETTINGS --version'
            	    	sh 'mvn -s $MAVEN_SETTINGS clean source:jar package'
	            }
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
			    configFileProvider(
                    [configFile(fileId: '327fa783-20f9-453c-8e5d-6404ee8c8ba8', variable: 'MAVEN_SETTINGS')]) {
                       	sh 'mvn -s $MAVEN_SETTINGS findbugs:findbugs'
	            }
				
			}
        }
        stage ('Package') {
			steps{
			    configFileProvider(
                    [configFile(fileId: '327fa783-20f9-453c-8e5d-6404ee8c8ba8', variable: 'MAVEN_SETTINGS')]) {
                       	sh 'mvn -s $MAVEN_SETTINGS source:jar package -Dmaven.test.skip'
	            }
				
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