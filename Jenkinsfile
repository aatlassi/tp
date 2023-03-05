node {
    def app
  
        stage('Checkout') {
            
                checkout([$class: 'GitSCM',
                          branches: [[name: 'main']],
                          userRemoteConfigs: [[url: 'https://github.com/aatlassi/tp.git']]])
        
        }
        stage('Create Staging Branch') {
                if (env.BRANCH_NAME=="staging"){
            
                    withCredentials([usernamePassword(credentialsId: '1a09e653-fe21-4449-a642-4f31aed8638d', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                
                     sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/aatlassi/tp.git')
                      }

                }else {
                    sh 'git checkout -b staging'
                    withCredentials([usernamePassword(credentialsId: '1a09e653-fe21-4449-a642-4f31aed8638d', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/aatlassi/tp.git')
                    }
                }

         stage('Test application') {
           
                  withPythonEnv('python3') {
                  sh 'python --version' 
                  sh 'pip install -r requirements.txt'
                  sh 'flask --app app run'
                  }     
            
        }

        stage('Build and push Docker Image'){

          app= docker.build("asmagr/tp-python:${env.BUILD_NUMBER}")
         
          docker.withRegistry('', 'dockerhubtp') {
            def customImage = docker.build("asmagr/tp-python:${env.BUILD_ID}")
            customImage.push()
           }
            echo "image built successfully"
        }
    
    
}

    