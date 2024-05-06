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
            sh 'mvn test --fail-never'
            sh 'mvn javadoc:jar --fail-never'
        }
    }
    stage('Build Docker Image') {
      steps {
          script {
              // 构建 Docker 镜像
              docker.build('teedy2024_manual')
          }
      }
    }

    stage('Upload Image') {
        steps {
            script {
                // 推送 Docker 镜像到 Docker Hub
                docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_credentials_id') {
                    docker.image('teedy2024_manual').push('latest')
                }
            }
        }
    }
    stage('Run containers') {
            steps {
                script {
                    // Pull Docker image from Docker Hub
                    docker.image('eedy2024_manual').pull()
                    
                    // Run Docker containers
                    docker.run('docker run -d -p 8084:8080 --name teedy_manual01 teedy2024_manual')
                    docker.run('docker run -d -p 8082:8080 --name teedy_manual02 teedy2024_manual')
                    docker.run('docker run -d -p 8083:8080 --name teedy_manual03 teedy2024_manual')
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
