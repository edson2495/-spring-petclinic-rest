pipeline {
    agent any
    tools {
        maven 'maven3.9.11'
    }
    
    stages {
        /*stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/edson2495/-spring-petclinic-rest.git'
            }
        }*/
        
        stage('Compile') {
            steps {
                sh 'mvn clean compile -B -ntp'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test -B -ntp'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -B -ntp -DskipTests'
            }
        }
    }
}