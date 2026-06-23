pipeline {
    agent {
        docker {
            // Using a standard Gradle/Java container to execute the tests
            image 'gradle:8.5-jdk17'
        }
    }

    stages {
        stage('Standard Tests') {
            steps {
                echo 'Running standard verification tasks from last week...'
                // Always runs regardless of file types changed
                sh './gradlew test' 
            }
        }

        stage('Code Coverage') {
            when {
                // Only executes if a .java file was modified in the commit(s)
                changeset '**/*.java'
            }
            steps {
                echo 'Java file change detected. Running Code Coverage...'
                sh './gradlew jacocoTestReport'
            }
        }

        stage('Checkstyle') {
            when {
                // Only executes if a .java file was modified in the commit(s)
                changeset '**/*.java'
            }
            steps {
                echo 'Java file change detected. Running Checkstyle analysis...'
                sh './gradlew checkstyleMain'
            }
        }
    }

    post {
        success {
            echo 'pipeline ran perfectly'
        }
        failure {
            echo 'pipeline failure'
        }
    }
}
