pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
	    	sh 'echo build'
                //sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
	    	sh 'echo test'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
	    	sh 'echo deploy'
               // sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
