pipeline {
  agent any
  stages {
    stage('Config System') {
      steps {
        echo 'Setup the system'
        sh 'sudo yum install wget -y'
        sh 'sudo yum install curl -y'
        sh 'sudo yum install java-1.8.0-openjdk'
        sh 'sudo wget http://dl.bintray.com/sbt/rpm/sbt-0.13.12.rpm'
        sh 'sudo yum localinstall sbt-0.13.12.rpm -y'
        sh 'sudo wget http://mirror.nohup.it/apache/spark/spark-2.2.0/spark-2.2.0-bin-hadoop2.7.tgz'
      }
    }
    stage('Test the System') {
      steps {
        sh 'java -version'
        sh 'sbt about'
      }
    }
    stage('Test scalatest') {
      steps {
        sh 'sbt clean test'
        archiveArtifacts 'target/test-reports/*.xml'
      }
    }
    stage('Build') {
      steps {
        sh 'sbt clean compile package'
      }
    }
    stage('Deploy') {
      steps {
        sh 'sudo cp target/scala-2.11/classes /opt/deploy/realTimeMovieRec/'
        sh 'sudo cp target/scala-2.11/*.jar /opt/deploy/realTimeMovieRec/'
        sh 'sudo cp target/webapp /opt/deploy/realTimeMovieRec/'
      }
    }
  }
}