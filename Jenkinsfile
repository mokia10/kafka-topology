pipeline {

    agent {
      docker { image 'purbon/kafka-topology-builder:latest' }
    }

   stages {
        stage ('Checkout') {
        checkout scm: [
            $class: 'GitSCM', 
            branches: [[name: '*/master']], 
            userRemoteConfigs: [[credentialsId: 'git-prv', url: 'https://github.com/mokia-unzer/kafka-topology.git']]]

        } 
        stage('run') {
          steps {
              withCredentials([usernamePassword(credentialsId: 'confluent-cloud', usernameVariable: 'CLUSTER_API_KEY', passwordVariable: 'CLUSTER_API_SECRET')]) {
                sh './demo/build-connection-file.sh > topology-builder.properties'
              }
              sh 'kafka-topology-builder.sh --clientConfig topology-builder.properties --topology ${TopologyFiles} --allowDelete'
          }
        }
   }
}
