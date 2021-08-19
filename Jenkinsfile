pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn -DskipTests -Pprod clean package'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -Pprod test'
        junit 'target/surefire-reports/*.xml'
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