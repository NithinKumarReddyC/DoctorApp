pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {

        stage('Build') {
            steps {
                echo "---------- Build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------- Build completed ----------"
            }
        }

        stage('Test') {
            steps {
                echo "---------- Unit test started ----------"
                sh 'mvn surefire-report:report'
                echo "---------- Unit test completed ----------"
            }
        }

        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'MySonarQubeScanner'
            }

            steps {
                echo "---------- SonarQube analysis started ----------"

                withSonarQubeEnv('SonarQube-Jenkins') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }

                echo "---------- SonarQube analysis completed ----------"
            }
        }
    }
}
