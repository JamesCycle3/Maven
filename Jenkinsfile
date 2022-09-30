pipeline{
      agent any
      stages{
            stage('check out'){
                  steps{
			sh "sudo su"
                        sh "rm -rf Maven"
                        sh "git clone https://github.com/anilpu3/Maven.git"
			sh "sudo chmod 777 /var/lib/"
			sh "apt install docker.io -y"
                  }
            }
            stage('build'){
                  steps{
                        sh "pwd"
                        sh "ls"
                        sh "cd Maven"
		                  	sh 'mvn clean install -DskipTests'
                        sh "echo ${BUILD_NUMBER}"
                        sh "docker build -t namma-maven-image:${BUILD_NUMBER} ."
                  }
            }

stage ('Push image to Artifactory') { // take that image and push to artifactory
        steps {
            rtDockerPush(
                serverId: "Namma-Jfrog",
                image: "namma-maven-image:${BUILD_NUMBER}",
                host: 'tcp://localhost:8082',
                targetRepo: 'libs-release-local', // where to copy to (from docker-virtual)
                // Attach custom properties to the published artifacts:
                properties: 'project-name=namma-project;status=stable'
            )
        }
    }
}
}
