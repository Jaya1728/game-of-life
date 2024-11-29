pipeline {
    agent any

    options {
        timeout(time: 30, unit: 'MINUTES')
    }

    triggers {
        cron('H */4 * * 1-5')
    }

    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['validate', 'package', 'test'], description: 'Select the Maven goal to execute')
    }

    tools {
        jdk 'JDK_8'
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
                    // Run the selected Maven goal
                    sh "mvn clean ${params.MAVEN_GOAL}"
                }
            }
        }

        stage('Test') {
            when {
                expression { params.MAVEN_GOAL == 'test' }
            }
            steps {
                archiveArtifacts artifacts: 'target/*.jar'
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
            }
        }
    }

    post {
        success {
            mail(
                to: 'sai@hi.com',
                subject: 'Build Success',
                body: 'Perfectly, my project was built successfully.'
            )
        }

        failure {
            mail(
                to: 'sai@hi.com',
                subject: 'Build Failure',
                body: 'The project failed to build due to errors.'
            )
        }
    }
}
