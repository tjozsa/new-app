pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-p 44419:44419 -v /root/.m2:/root/.m2 --network=docker_cd-tools-network' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn clean package' 
            }
        }
        stage('Test') {
            steps {
               echo 'Run tests here...'
            }
        }
        stage('Publish Artifact') {
            steps {
                withEnv(["ARTIFACTORY_SERVER_URL=${env.ARTIFACTORY_SERVER}", "ARTIFACTORY_LOGIN=admin", "ARTIFACTORY_PASSWORD=password", "ARTIFACT=spincast-todobackend-inmemory-1.0.2.jar"]) {
                    sh 'curl -u ${ARTIFACTORY_LOGIN}:${ARTIFACTORY_PASSWORD} -X PUT "${ARTIFACTORY_SERVER_URL}/artifactory/todobackend-java/${ARTIFACT}" -T /var/jenkins_home/workspace/todobackend-java/spincast-todobackend-inmemory/target/${ARTIFACT}'
                }
            }
        }
        stage('Deploy') {
            steps {
              sh 'java -jar /var/jenkins_home/workspace/todobackend-java/spincast-todobackend-inmemory/target/spincast-todobackend-inmemory-1.0.2.jar &'
            }
        }
    }
}
