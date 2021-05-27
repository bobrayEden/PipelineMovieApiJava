pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk 'JDK11'
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git url: 'https://github.com/matthcol/movieapijava2021.git',
                    branch: 'dev'
                // Run Maven on a Unix agent.
                sh "mvn clean compile"
                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage ('Test') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true test"
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
                failure {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    sh "exit 1"
                    error "Failed, exiting now"
                }
                unstable {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    sh "exit 1"
                    error "Unstable, exiting now"
                }
            }
        }
        stage('Package') {
            steps {
                sh "mvn -DskipTests package"
            }
        }
    }
}