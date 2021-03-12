pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'JDK11'
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
        stage('Upload'){
            steps {
                rtUpload (
                    serverId: 'artifactory',
                    spec: '''{
                        "files": [
                            {
                            "pattern": "bazinga/*froggy*.zip",
                            "target": "bazinga-repo/froggy-files/"
                            }
                        ]
                    }''',
                
                    // Optional - Associate the uploaded files with the following custom build name and build number,
                    // as build artifacts.
                    // If not set, the files will be associated with the default build name and build number (i.e the
                    // the Jenkins job name and number).
                    // buildName: 'holyFrog',
                    // buildNumber: '42'
                )
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
