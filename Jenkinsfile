pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage("build") {
            steps {
                echo "---------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------- build completed ----------"
            }
        }

        stage("test") {
            steps {
                echo "---------- unit test started ----------"
                sh 'mvn surefire-report:report'
                echo "---------- unit test completed ----------"
            }
        }

        // Note: The image cuts off at the start of the 'SonarQube analysis' stage
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'MySonarQubeScanner'
                        // Additional SonarQube environment variables go here
            }
                steps {
                        withSonarQubeEnv('SonarQube-Jenkins'){
                        sh "${scannerHome}/bin/sonar-scanner"
                }
        }
    }
}
	
