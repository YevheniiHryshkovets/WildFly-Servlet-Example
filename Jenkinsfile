pipeline {
agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Checkout') {
            steps {
                
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/XadmaX/WildFly-Servlet-Example.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean install"

                sh 'ls -ltrah target/.'
                echo "maven successfull build"
            }
        }

        stage('Deploy') {
            // deploy
            steps {
                sh 'whoami'
                sh 'ls -ltrah /var/lib/jenkins/workspace/wildFlyDemo/BuildServerExample/target/.'
                sh 'scp -i /home/jenkins/.ssh/from_jenkins_to_wildfly_key /var/lib/jenkins/workspace/wildFlyDemo/BuildServerExample/target/devops-1.0-SNAPSHOT.war wildfly@34.228.141.40:/opt/wildfly/standalone/deployments/'
            }
        }
    }

    post {
            // If Maven was able to run the tests, even if some of the test
            // failed, record the test results and archive the jar file.
        success {
            echo "-----SUCCESS-----"
            // Clear Workspace
            cleanWs()
        }

        failure {
            echo "-----FAIL-----"
            // Clear Workspace
            cleanWs()
        }
    }
}
