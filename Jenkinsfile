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

        def customImage = docker.build("abhishek99868/puji:${env.BUILD_ID}")

        /* Push the container to the custom Registry */
        customImage.push()
          }

         echo "Trying to push Docker Build to DockerHub"
       }
 stage('Pull image') {

        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {

        def customImage = docker.build("abhishek99868/puji:${env.BUILD_ID}")

        /* Push the container to the custom Registry */
        customImage.pull()
		      customImage.run()
          }
          }
    }
