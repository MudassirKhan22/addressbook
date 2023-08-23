pipeline {
    agent any

    tools {
        
        maven "mymaven"
    }

    stages {
        stage('Compile') {
            steps {
                
                git 'https://github.com/MudassirKhan22/addressbook.git'

                
                sh "mvn compile"

                
            }

           
        }
        
         stage('UnitTest') {
            steps {
                
                sh "mvn test"

            }

           
        }
        
         stage('Package') {
            steps {
                
               

                
                sh "mvn package"

            }

           
        }
    }
}
