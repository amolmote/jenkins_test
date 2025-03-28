pipeline {
    agent any
    
    tools {
        maven "MVN3"
    }
    
    stages {
        stage ('pull scm') {
            steps {
                git credentialsId: 'github', url: 'git@github.com:amolmote/jenkins_test.git'
            }
        }
        
        stage('build') {
            steps {
                sh "mvn -f api-gateway/ clean package"
            }
        }
        
        stage('publish') {
            steps {
                archiveArtifacts artifacts: 'api-gateway/target/*.jar', followSymlinks: false
                junit stdioRetention: '', testResults: 'api-gateway/target/surefire-reports/*.xml'
            }
        }
        
        stage ('print') {
            steps {
                sh "echo testing"
            }
        }
    }
}
