pipeline {
    agent any
    environment {
        gpg_secret = credentials("gpg-secret")
        gpg_trust = credentials("gpg-ownertrust")
        gpg_passphrase = credentials("gpg-passphrase")
    }
    stages { 
        stage("Install Git Secret") {
            steps {
                sh """
                    echo "deb https://dl.bintray.com/sobolevn/deb git-secret main" | tee -a /etc/apt/sources.list
                    wget -qO - https://api.bintray.com/users/sobolevn/keys/gpg/public.key | apt-key add -
                    apt-get update && apt-get install git-secret
                """
            }
        }
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
