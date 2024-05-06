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

    stage('Build Docker Image') {
      steps {
            script {
                // 构建 Docker 镜像
                sh 'docker build teedy2024_manual .'
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
                  sh 'docker run -d -p 8084:8080 --name teedy_manual01 teedy2024_manual'
                  sh 'docker run -d -p 8082:8080 --name teedy_manual02 teedy2024_manual'
                  sh 'docker run -d -p 8083:8080 --name teedy_manual03 teedy2024_manual'
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
