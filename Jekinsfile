pipeline{
    agent any
    tools { 
        maven 'MAVEN_HOME' 
      }
    stages {
      stage('Build'){
        steps{
          sh 'mvn -B -DskipTests clean package'
        }
      }
      stage('pmd'){
        steps{
          sh 'mvn pmd:pmd'
        }
    }
      stage('test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Generate Surefire Report') {
            steps {
                sh 'mvn surefire-report:report'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/site/surefire-report.html', fingerprint: true
                }
            }
        }
        stage('Generate Javadoc') {
            steps {
                sh 'mvn javadoc:jar'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/site/apidocs/*', fingerprint: true
                }
            }
        }
  }
  post{
    always{
      archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
      archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
      archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
    }
  }
}
