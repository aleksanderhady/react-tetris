pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git url: 'https://github.com/aleksanderhady/react-tetris'
                sh 'npm install'
                sh 'git pull origin master'
            }
            
            post {
                failure {
                    script {
                        env.FAILED = true
                    }  

                    mail to: 'aleksanderhady@gmail.com',
                        subject: "Failure on building ${currentBuild.fullDisplayName}",
                        body: "Failure on building ${env.BUILD_URL} "      
                      
                }
                success {
                    echo 'Build success!!!'
				    mail to: 'aleksanderhady@gmail.com',
                        subject: "Success on building ${currentBuild.fullDisplayName}",
                        body: "Success on building ${env.BUILD_URL} " 
                }
            }
        }
        stage('Test') {
             steps {
                 script {
                    if ( env.FAILED ) {
                    expression {
                        currentBuild.result = 'ABORTED'
                        error('Failed on build stage!')
                        }
                    }
                }
            }
            post {
                failure {
                     mail to: 'aleksanderhady@gmail.com',
                        subject: "Failure on testing ${currentBuild.fullDisplayName}",
                        body: "Failure on testing ${env.BUILD_URL} "    
                }
                success {
                    mail to: 'aleksanderhady@gmail.com',
                        subject: "Success on testing ${currentBuild.fullDisplayName}",
                        body: "Success on testing ${env.BUILD_URL} "    
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker build -t dockertetris -f Dockerfile .'
                sh 'docker run dockertetris'
                }
                post {
                    failure {
                        mail to: 'aleksanderhady@gmail.com',
                            subject: "Failure deploy: ${currentBuild.fullDisplayName}",
                            body: "Failure deploy ${env.BUILD_URL} "         
                    }
                    success {
                        mail to: 'aleksanderhady@gmail.com',
                            subject: "Success deploy: ${currentBuild.fullDisplayName}",
                            body: "Success deploy ${env.BUILD_URL} "                        
                    }
                }
        }
    }
}
