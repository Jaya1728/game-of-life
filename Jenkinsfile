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
     stages {
         stage('git') {
            steps { 
            git url : 'https://github.com/Jaya1728/game-of-life.git',
                branch : 'master'
     }
         }

        stage('build and package ') {
            steps { 
                 parameters {
            choice(name: 'mvn', choices: ['validate', 'package', 'test'], description: 'Pick something')
     }
        }

        stage('Test ') {
            steps { 
            archiveArtifacts artifacts : '**/target/game-of-life-*.jar'
            junit testResults :  '**/target/surefire-reports/TEST-*.xml'
     }
        }
    
}


}