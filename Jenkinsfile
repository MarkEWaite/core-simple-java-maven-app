pipeline {
  agent {
    kubernetes {
      label 'maven-pod-template'
      defaultContainer 'maven-container'
      yamlFile 'KubernetesPod.yaml'
    }
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    // stage('Test') {
    //   steps {
    //     sh 'mvn test'
    //   }

     stage('JDK 11 Build & Test') {
       steps {
         container('maven-container-jdk-11') {
         sh 'mvn --version'
         sh 'mvn -B clean package'
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
