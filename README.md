<img width="1908" height="944" alt="jenkins 1" src="https://github.com/user-attachments/assets/37d65f6c-60bb-44ec-b42f-c229680a28af" />

<img width="1908" height="934" alt="jenkins3" src="https://github.com/user-attachments/assets/2e753189-3652-4f83-99ca-559cd3139feb" />


<img width="1919" height="1011" alt="jenkins2" src="https://github.com/user-attachments/assets/365c26a0-4ac3-49e5-be70-7863d018ab5b" />


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
