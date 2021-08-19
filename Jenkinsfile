pipeline {
  agent any
  stages {
    stage('Build') {
      agent{
        docker{
          image 'maven:3.8.1'
        }
      }
      steps {
        sh 'mvn -DskipTests -Pprod clean package'
      }
    }

    stage('Test') {
      agent{
        docker{
          image 'maven:3.8.1'
        }
      }
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
        sh 'docker build -f Dockerfile-mysql -t notweibo/mysql'
        sh 'docker build -f Dockerfile-app -t notweibo/app'
        sh 'docker-compose up -d'
      }
    }

  }
}