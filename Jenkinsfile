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
        stage('Publishing'){
            steps {

                echo 'Publishing...'

				rtPublishBuildInfo (
    				serverId: 'artifactory',
    			)

				rtUpload (
				    serverId: 'artifactory',
				    spec: '''{
				          "files": [
				            {
				              "pattern": "target/spring-petclinic*.jar",
				              "target": "PetClinicApp/spring-petclinic-${BUILD_NUMBER}.jar"
				            }
				         ]
				    }'''
				)
                
            }
        }
        stage('Deliver') {
            steps {
	    	    echo "Beginning Deployment..."

                sshagent(['demokey']) {
                    // some block
                    sh """ssh -tt -o StrictHostKeyChecking=no ubuntu@10.0.1.6 << EOF 
cat /etc/hostname
datetime
cd petclinic/
curl -uadmin:AP7P4GgnmSn1KrmbYajgMT7ssBd -o spring-petclinic.jar --no-progress-meter "http://40.84.217.254:8081/artifactory/PetClinicApp/spring-petclinic-${BUILD_NUMBER}.jar"
sudo systemctl restart petclinic.service
exit
                    EOF"""   
                }

               echo "Deployment Complete!"
            }
        }
    }
}
