
<img width="1908" height<img width="1924" height="935" alt="jenkins4" src="https://github.com/user-attachments/assets/7c7258fb-0128-4337-9053-4e4ede6953e7" />


<img width="1923" height="939" alt="jenkins3" src="https://github.com/user-attachments/assets/fc39cfd0-2064-4a1a-8285-76c41251de19" />


##  Mon Script :
Mon Script :
pipeline {
    agent any

    tools {
        jdk 'jdk-17'
        maven 'maven'
    }

    stages {

        stage('Git Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/EttouyjerYasmine/first_test_jenkins.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build') {
            steps {
                dir('POV-JAVA') {
                    bat 'mvn clean install'
                }
            }
        }

        stage('Create Docker Image') {
            steps {
                dir('POV-JAVA') {
                    bat 'docker build -t pos-app .'
                }
            }
        }

        stage('Clean old container') {
            steps {
                bat 'docker rm -f pos-app || exit 0'
            }
        }

        stage('Run') {
            steps {
                dir('POV-JAVA') {
                    bat 'docker run --name pos-app -d -p 8585:8282 pos-app'
                }
            }
        }
    }
}
