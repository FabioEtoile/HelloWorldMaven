pipeline {
    agent any 
    tools {
        maven 'maven-3.9.11' 
    }

    triggers {
        pollSCM '* * * * *'
    }

    // différentes étapes
    stages {
        
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/FabioEtoile/HelloWorldMaven.git',
                    credentialsId: 'fad0579a-75c0-44db-a002-23b4df1cc62d'
            }
        }

        stage('Build Maven') {
            steps {
                // Comme on a déclaré l'outil en haut, on peut juste taper 'mvn' !
                sh 'mvn clean install'
            }
        }

        stage('Tag & Push') {
            steps {
                // On sécurise les identifiants
                withCredentials([usernamePassword(credentialsId: 'fad0579a-75c0-44db-a002-23b4df1cc62d', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    script {
                        sh """
                            echo "--- Config Git ---"
                            git config user.email "jenkins@gmail.com"
                            git config user.name "Jenkins"

                            echo "--- Tagging build-${BUILD_NUMBER} ---"
                            git tag -a -f build-${BUILD_NUMBER} -m "Tag du build ${BUILD_NUMBER}"

                            echo "--- Pushing ---"
                            git push https://${GIT_USER}:${GIT_PASS}@github.com/FabioEtoile/HelloWorldMaven.git --tags
                        """
                    }
                }
            }
        }
    }
}
