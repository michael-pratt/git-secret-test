pipeline {
    agent any
    environment {
        gpg_secret = credentials("gpg-secret")
        gpg_trust = credentials("gpg-trust")
        gpg_passphrase = credentials("gpg-passphrase")
    }
    stages { 
        stage("Import GPG Keys") {
            steps {
                sh """
                    gpg --batch --import $gpg_secret
                    gpg --import-ownertrust $gpg_trust
                """
            }
        }
        stage("Reveal Git Secrets") {
            steps {
                sh """
                    cd $WORKSPACE
                    git init
                    git-secret reveal -p '$gpg_passphrase'
                """
            }
        }
    }
}
