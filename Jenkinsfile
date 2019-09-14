node('master') {

stage ('checkout code'){
	checkout scm
}
	
stage ('Build'){
	sh "mvn clean install"
}

stage ('Test Cases Execution'){
	sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
}

stage ('Sonar Analysis'){
	//sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:80'
}

stage ('Archive Artifacts'){
	archiveArtifacts artifacts: 'target/*.war'
}
	
stage ('Deployment'){
	sh 'docker build -t javdocker .'
}

stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push javdocker'
        }
      }
    }
}
