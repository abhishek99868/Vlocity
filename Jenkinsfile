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
		}
		}
stage('Create_Docker') {
    steps{
    echo 'This job creates docker.'
    sh '''
set +x
cat << EOF > compose_vlocity_tool.yml
vlocity_build_tool:
  container_name: vlocity-build-tool
  restart: always
  def customImage = docker.build("abhishek99868/puji:${env.BUILD_ID}")
  net: local_network
  privileged: true
  environment:
    SLAVE_LABELS: "vlocity buildvlocity"
    SWARM_PASSWORD: ${SWARM_PASSWORD}
    SLAVE_EXECUTORS: 2
    INITIAL_ADMIN_USER: ${INITIAL_ADMIN_USER}
EOF
docker-compose -f compose_vlocity_tool.yml up -d
set -x
'''
          }
          }
		  
