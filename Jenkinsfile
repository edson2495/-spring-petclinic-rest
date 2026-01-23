pipeline {
    /*agent any
    tools {
        maven 'maven3.9.11'
    }*/

    agent {
        docker {
            image 'maven:3.9.12-eclipse-temurin-17'
            //args '-v /root/.m2:/root/.m2'
        }
    }

    triggers{
        pollSCM("* * * * *")
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