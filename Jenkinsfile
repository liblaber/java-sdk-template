pipeline {
    agent {
        label 'ubuntu-latest'
    }
    environment {
        MAVEN_USERNAME = credentials('MAVEN_USERNAME')
        MAVEN_PASSWORD = credentials('MAVEN_PASSWORD')
        GPG_PRIVATE_KEY = credentials('GPG_PRIVATE_KEY')
        GPG_PASSPHRASE = credentials('GPG_PASSPHRASE')
    }
    stages {
        stage('Run Only on Tag') {
            when {
                expression {
                    // Matches tags like rls_0.1.0, rls_0.1.1, etc.
                    return env.BRANCH_NAME ==~ /^rls_\d+\.\d+\.\d+$/  
                }
            }
            steps {
                echo "Running on tag: $env.BRANCH_NAME"
            }
            stages {
                stage('Pre-check for Environment Variables') {
                    steps {
                        script {
                            if (!env.MAVEN_USERNAME || !env.MAVEN_PASSWORD || !env.GPG_PRIVATE_KEY || !env.GPG_PASSPHRASE) {
                                error("Error: One or more required environment variables (MAVEN_USERNAME, MAVEN_PASSWORD, GPG_PRIVATE_KEY, GPG_PASSPHRASE) are not defined.")
                            }
                            echo "All necessary environment variables are defined."
                        }
                    }
                }
                stage('Checkout Code') {
                    steps {
                        checkout scm
                    }
                }
                stage('Install GPG Secret Key') {
                    steps {
                        sh '''
                            echo "$GPG_PRIVATE_KEY" | gpg --batch --import
                        '''
                    }
                }
                stage('Set up Maven') {
                    steps {
                        sh '''
                            # Install Java 8 and Maven for deployment to Maven Central
                            sudo apt-get update
                            sudo apt-get install -y openjdk-8-jdk maven
                        '''
                    }
                }
                stage('Publish to Maven Central') {
                    steps {
                        sh '''
                            mvn --no-transfer-progress --batch-mode \
                            -Dgpg.passphrase=$GPG_PASSPHRASE \
                            clean deploy -P release-sign-artifacts -e
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
