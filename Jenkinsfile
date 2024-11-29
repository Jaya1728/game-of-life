pipeline {
    agent any

    options {
        timeout(time: 30, unit: 'MINUTES')
    }

    triggers {
        cron('H */4 * * 1-5')
    }

    tools {
        jdk 'JDK_8'
    }

    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['validate', 'package', 'test'], description: 'Select the Maven goal to execute')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Jaya1728/game-of-life.git', branch: 'master'
            }
        }

        stage('Build and Package') {
            steps {
                
     sh script : "mvn ${params.MAVEN_GOAL}"
                }
            }
        

        stage('Test') {
           
            steps {
            
                archiveArtifacts artifacts: '**/target/game-of-life-*.jar',
                junit testResults: '**/target/surefire/TEST-*.xml'
            }
        }
    }
 post {
    success {
        mail subject : "your project is success"
        body         : "perfectly my project was build"
        to           : "sai@hi.com"
    }
 failure {
        mail subject : "your project is failure"
        body         : "my project was not build due to errors "
        to           : "sai@hi.com"
    }
 
 }
}
