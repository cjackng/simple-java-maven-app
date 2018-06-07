pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
				}		
        }
		stage('Scan') {
            steps {
                nexusPolicyEvolution failBuildOnNetworkError:false, iqApplication: "mvn-sample",
				iqScanPattern:[[scanPattern:"**/*jar"]], iqStage: 'build', jobCredentialsId:''
            }
        }
		stage('Test') {
            steps {
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
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}