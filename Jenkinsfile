node('docker') {
 def app
 stage('Clone repository') {
       /* Cloning the Repository to our Workspace */

       checkout scm

      }
 
 stage('Build image') {
       /* This builds the actual image */

       app = docker.build("yashdvs/vlocity")

      }

 stage('Push image') {
         /*
             You would to first register with Dockerhub before you can push images to your account
              */

        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {

        def customImage = docker.build("yashdvs/vlocity:${env.BUILD_ID}")

        /* Push the container to the custom Registry */
        customImage.push()
        }
  stage('Pull image') {
         /*
             You would to first register with Dockerhub before you can push images to your account
              */

        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {

        def customImage = docker.build("yashdvs/vlocity:${env.BUILD_ID}")
        /* Pull the docker image from the custom Registry */
        
        customImage.pull()
        stage('Docker Run') {
        customImage.run()
          }
          }

         echo "The docker image is running now"
                     }
                     }
 }
