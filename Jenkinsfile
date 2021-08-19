pipeline {
  agent none
  stages {
    stage('Build and Test') {
      agent {
        docker {
          image 'maven:3.8.1-adoptopenjdk-11'
        }

      }
      steps {
        sh 'mvn -DskipTests -Pprod clean package'
        sh 'mvn -Pprod test'
        stash includes: 'target/*.jar', name: 'jar'
      }
      post{
        always{
          junit 'target/surefire-reports/*.xml'
        }
      }
    }


    stage('Deploy to Docker') {
      agent any
      steps {
        unstash 'jar'
        sh 'docker build -f Dockerfile-mysql -t notweibo/mysql .'
        sh 'docker build -f Dockerfile-app -t notweibo/app .'
        sh 'docker-compose up -d'
      }
    }

  }
}