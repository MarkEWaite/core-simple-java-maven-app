pipeline {
  agent {
    kubernetes {
      label 'mypodtemplate-maven-3.5.4'
      containerTemplate {
        name 'maven'
        image 'maven:3.5.4-jdk-8-alpine'
        ttyEnabled true
        command 'cat'
      }
    }
  }
  stages {
    stage('Work') {
      parallel {
        stage('Build') {
          steps {
            sh 'mvn -B -DskipTests clean package'
          }
        }
        stage('Test') {
          steps {
            sh 'mvn test'
          }
          post {
            always { 
              junit '**/*.xml'
            }
          }
        }
        stage('Deliver') {
          steps {
            sh 'jenkins/scripts/deliver.sh'
          }
        }
      }
    }
  }
}
