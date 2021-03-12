pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'JDK8'
    }
    stages {
        stage('Build') {
            steps {
                withMaven(maven: 'maven'){
                    sh 'echo build'
                    sh 'mvn -B -DskipTests clean package'
                }	    	    
            }
        }
        stage('Test') {
            steps {
	    	    withMaven(maven: 'maven'){
                    sh 'echo test'
                    sh 'mvn -B test'
                }
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
