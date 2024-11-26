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
                script {
                    // Using the selected Maven goal
                    def mvnCommand = "mvn clean ${params.MAVEN_GOAL}"
                    sh mvnCommand
                }
            }
        }

        stage('Test') {
            when {
                expression { params.MAVEN_GOAL == 'test' }
            }
            steps {
                script {
                    // Run tests and collect results
                    sh 'mvn test'
                }
                archiveArtifacts artifacts: '**/target/game-of-life-*.jar', fingerprint: true
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
            }
        }
    }
}
