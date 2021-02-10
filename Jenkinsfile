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
                shell ".\gradew clean"
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

def shell(command) {
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
