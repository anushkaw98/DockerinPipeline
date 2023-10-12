# DockerinPipeline
create a pipeline to add Docker file into dockerhub using Jenkins



<h3>prerequisites</h3>
- please check below repository,it will hepls to create docker image(nodejs application) which use here to create pipeline
- Go to manage credentials and add Credentials for 

  - <b>Docker</b> 
  - <b>Github</b> 

 #### (https://github.com/anushkaw98/nodejs_Docker.git)



<h2>Environments Used </h2>

- <b>Jenkins</b>
- <b>Docker desktop</b>

<h2>Program walk-through:</h2>

Go to Jekins dashboard and create a new pipeline



set a Poll SCM Schedul(if need) under Build Triggers section

enter below code insude to the pipeline script


    pipeline {
        agent any
        environment {
            dockerImage = ''
            registry = 'anushkaw98/nodejs'
            registryCredential ='dockerhub'
      }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/anushkaw98/nodejs_Docker.git']]])
            }
        }

        stage('Build Docker image') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        
        stage("uploading image"){
            steps{
                script{
                    docker.withRegistry( '', registryCredential ){
                    dockerImage.push()
                    }
                }
                    
                }
                
            }
        }
        
        
        
    }




