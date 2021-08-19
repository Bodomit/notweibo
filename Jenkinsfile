pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3.8.1-adoptopenjdk-11'
          args '-v $HOME/.m2:/root/.m2'
        }

      }
      steps {
        sh 'mvn -DskipTests -Pprod clean package'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'maven:3.8.1-adoptopenjdk-11'
          args '-v $HOME/.m2:/root/.m2'
        }

      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }

      }
      steps {
        sh 'mvn -Pprod test'
        stash includes 'target/*.jar', name: 'jar'
      }
    }

    stage('Deploy to Docker') {
      agent none
      steps {
        unstash 'jar'
        sh 'docker build -f Dockerfile-mysql -t notweibo/mysql .'
        sh 'docker build -f Dockerfile-app -t notweibo/app .'
        sh 'docker-compose up -d'
      }
    }

  }
}