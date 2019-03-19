node {
 def app
 stage('Clone repository') {
       /* Cloning the Repository to our Workspace */

       checkout scm

      }
 
 stage('Build image') {
       /* This builds the actual image */

       app = docker.build("abhishek/puji")

      }

 stage('Push image') {
         /*
             You would to first register with Dockerhub before you can push images to your account
              */
    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
       app.push("${env.BUILD_NUMBER}")
       app.push("latest")
          }

         echo "Trying to push Docker Build to DockerHub"
       }
 }
