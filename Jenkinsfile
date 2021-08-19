pipeline {
  agent{
        docker{
          image 'maven:3.8.1-adoptopenjdk-11'
          args '-v /root/.m2:/root/.m2'
        }
      }
  stages {
    stage('Build') {
      
      steps {
        sh 'mvn -DskipTests -Pprod clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn -Pprod test'
      }
      post{
        always{
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Deploy to Docker') {
      steps {
        sh 'docker build -f Dockerfile-mysql -t notweibo/mysql .'
        sh 'docker build -f Dockerfile-app -t notweibo/app .'
        sh 'docker-compose up -d'
      }
    }

  }
}