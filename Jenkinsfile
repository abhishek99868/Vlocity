pipeline{
    agent any
    stages{
    stage('Create_Docker') {
    steps{
    echo 'This job creates docker.'
    sh '''
        set +x 
        rm -rf Dockerfile
        cat << EOF > Dockerfile
        FROM centos:latest
        RUN yum update -y
        RUN curl -sL https://rpm.nodesource.com/setup_8.x | bash -
        RUN yum install nodejs -y
        RUN yum install -y gcc-c++ make curl wget git openldap-clients openssl libxslt -y && yum clean all

        ENV JAVA_VERSION=1.8.0_131

        ENV JAVA_TARBALL=server-jre-8u131-linux-x64.tar.gz

        ENV JAVA_HOME=/opt/java/jdk${JAVA_VERSION}
        ENV SWARM_MASTER=http://jenkins:8080/jenkins/
        ENV SWARM_USER=jenkins
        ENV SWARM_PASSWORD=jenkins
        ENV SLAVE_NAME="vlocity_slave"
        ENV SLAVE_LABELS="vlocity vlocity_build_tool"
        ENV SLAVE_MODE="exclusive"
        ENV SLAVE_EXECUTORS=1
        ENV SLAVE_DESCRIPTION="vlocity_build_tool hosted slave"

        RUN npm install --global vlocity

        RUN wget --no-check-certificate --no-cookies --directory-prefix=/tmp --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz && mkdir -p /opt/java && tar -zxf /tmp/jdk-8u131-linux-x64.tar.gz -C /opt/java &&     /usr/sbin/alternatives --install /usr/bin/java java /opt/java/jdk1.8.0_131/bin/java 100 && rm -rf /tmp/* && rm -rf /var/log/*

        RUN curl -s -o /bin/swarm-client.jar -k http://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/2.0/swarm-client-2.0-jar-with-dependencies.jar
        CMD /opt/java/jdk1.8.0_131/bin/java -jar /bin/swarm-client.jar -executors 2 -description "vlocity_build_tool hosted slave" -master http://jenkins:8080/jenkins/ -username ${SWARM_USER} -password ${SWARM_PASSWORD} -name "vlocity_slave" -labels "vlocity buildvlocity" -mode ${SLAVE_MODE}
      
        EOF
        docker build -t vlocity_build .

        cat << EOF > compose.yml
        vlocity-build:
            container_name: vlocity-build
            restart: always
            image: vlocity-build:latest
            net: local_network
            privileged: true
            environment:
                SLAVE_LABELS: "vlocity buildvlocity"
                SWARM_PASSWORD: ${SWARM_PASSWORD}
                SLAVE_EXECUTORS: 2
                INITIAL_ADMIN_USER: ${INITIAL_ADMIN_USER}
        EOF
        docker-compose -f compose.yml up -d
        set -x
    '''
    }
    }
    stage('Push docker Image') {
    steps {
    echo 'Pushing docker Image.'
    sh '''
        set +x
         docker login -u yashdvs -p Qazxcdews159 -e yaswanth.dvs@gmail.com
         docker push yashdvs/vlocity-build
        set -x
    '''
    }
    }
    }
    }
