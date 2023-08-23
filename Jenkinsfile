pipeline {
    agent any

    tools {
        
        maven "mymaven"
    }


    parameters{
        string(name:'Env',defaultValue:'Test',description:'Version to deploy')
        booleanParam(name:'exceuteTests',defaultValue:true,description:'decide to run tc')
        choice(name:'APPVERSION',choices:['1.1','1.2','1.3'])



    }

    stages {
        stage('Compile') {

            steps {
                
                git 'https://github.com/MudassirKhan22/addressbook.git'

                
                sh "mvn compile"
                echo "Env to deploy: ${params.Env}"

                
            }

           
        }
        
         stage('UnitTest') {

            when{
                expression{
                    params.exceuteTests==true
                }
            }
            steps {
                
                sh "mvn test"

            }

           
        }
        
         stage('Package') {

            agent {label 'Linux-slave'}


            // when{
            //     expression{
            //         BRNACH_NAME == 'dev' || BRNACH_NAME == 'develop'
            //     }
            // }
            steps {
                
                sh "mvn package"
                echo "Deploying app version: ${params.APPVERSION}"

            }
        }

        stage('Deploy'){
            agent {label 'Linux-slave'}

            steps{
                input{
                    message: "Please approve to deploy"
                    ok "yes, to deploy"
                    parameters{
                        choice(name:'APPVERSION', choice['1.2','1.3','1.4'])
                    }
                }

                echo "Deploying to test"
            }
        }
    }
}
