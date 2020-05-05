pipeline {
    agent any
    tools {
        maven "M3"
    }
    stages {
        stage ('Clone the project') {
            steps {
                git 'https://github.com/Kaizenko/vending-machine.git'
            }
        }
        stage ('Compilation and Analysis') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test -Punit-tests'
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*UnitTest.xml'
                }
            }
        }
        stage ('Integration Tests') {
            steps {
                bat 'mvn failsafe:integration-test -Pintegration-tests'
            }
            post {
                always {
                    junit '**/target/failsafe-reports/TEST-*IntegrationTest.xml'
                }
            }
        }
        stage ('Acceptance Tests') {
            steps {
                bat 'mvn failsafe:integration-test -Pacceptance-tests'
            }
            post {
                always {
                    junit '**/target/failsafe-reports/TEST-*AcceptanceTest.xml'
                }
            }
        }
        //stage ('Browser Tests') {
          //  steps {
            //    bat 'mvn failsafe:integration-test -Pbrowser-tests'
            //}
            //post {
              //  always {
                //    junit '**/target/failsafe-reports/TEST-*BrowserTest.xml'
                //}
            //}
        //}
        stage('Promotion') {
            steps {
                timeout(time: 1, unit:'DAYS') {
                    input 'Deploy to Production?'
                }
            }
        }
        stage('Deploy to Production') {
            environment {
                HEROKU_API_KEY = credentials('HEROKU_API_KEY')
            }
            steps {
                bat 'mvn heroku:deploy'
            }
        }
    }
}
 
