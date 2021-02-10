pipeline {
    agent any

    stages {
        stage("Send GitHub build signal") {
            steps {
                echo "Signal sent"
            }
        }

        stage("Clean") {
            steps {
                shell ./gradew clean
                echo "clean complete"
            }
        }

        stage("Build") {
            parallel {
                stage("Assemble") {
                    steps {
                        echo "assemble successful"
                    }
                }

                stage("Lint") {
                    steps {
                        echo "linting successful"
                    }
                }
            }
        }

        stage("Test") {
            parallel {
                stage("Unit") {
                    steps {
                        shell ./gradlew testDebugUnitTest
                    }
                }

                stage("Integration") {
                    steps {
                        echo "integration test successful"
                    }
                }
            }
        }
    }

    post {
        unsuccessful {
            sendMail("subm", "kdkdk")
        }
        fixed {
            sendMail("Fixed build", "blah")
        }
    }
}

def shell() {
    if (isUnix()) {
        sh command
    } else {
        bat command
    }
}

def sendMail(subject, body) {
    emailext subject: subject,
        recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
        body: body
}
