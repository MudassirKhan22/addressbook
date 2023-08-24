pipeline {
    agent any

    tools {
         
        maven "mymaven"
    }

    environment{
        BUILD_SERVER_IP='ec2-user@172.31.42.99'
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

            agent any


            // when{
            //     expression{
            //         BRNACH_NAME == 'dev' || BRNACH_NAME == 'develop'
            //     }
            // }
            steps {
                script{
                    sshagent(['build-server-key']){
                        echo "Packaging the code on new slave"
                        sh "scp -o StrictHostKeyChecking=no server-config.sh ${BUILD_SERVER_IP}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${BUILD_SERVER_IP} 'bash ~/server-config.sh'"
                    }

                }
                
                

            }
        }

        stage('Deploy'){
            agent {label 'Linux-slave'}

            input{
                    message "Please approve to deploy"
                    ok "yes, to deploy"
                    parameters{
                        choice(name:'NEWVERSION', choices:['1.2','1.3','1.4'])
                    }
                }

            steps{
                

                echo "Deploying to test"
            }
        }
    }
}
