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
    options{
        timeout(time: 5, unit: 'MINUTES')
    }
    triggers{
        //pollSCM("* * * * *") //cada minuto
        githubPush()
    }
    
    stages {
        /*stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/edson2495/-spring-petclinic-rest.git'
            }
        }*/
        
        stage('Compile') {
            steps {
                //sh 'mvn clean compile -B -ntp'
                withEnv(['MAVEN_OPTS=-Xms256m -Xmx1024m -XX:+UseG1GC']) {
                    sh 'mvn clean compile -B -ntp'
                }
            }
        }
        
        stage('Test') {
            steps {
                // sh 'mvn test -B -ntp'
                sh 'mvn test -Dmaven.test.failure.ignore=true -B -ntp'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Coverage') {
            steps {
                sh 'mvn jacoco:report -B -ntp'
            }
            post {
                success {
                    recordCoverage(tools: [[parser: 'JACOCO']])
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -B -ntp -DskipTests'
            }
        }
        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonarqube') { //con el mismo nombre que creo el server en jenkins lo llama
                    sh 'mvn sonar:sonar -B -ntp'
                }
            }
        }
    }
    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        cleanup{
            cleanWs()
        }
    }
}