pipeline {
    agent any

    tools {
         
        maven "mymaven"
    }

    environment{
        BUILD_SERVER_IP='ec2-user@172.31.42.99'
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

            steps {
                
                sh "mvn test"

            }

            post{
                always{
                    junit 'target/surfire-reports*.xml'
                }
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
