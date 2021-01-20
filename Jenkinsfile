podTemplate(
    name: 'p3-kafka-topology-builder',
    serviceAccount : 'jenkins',
    containers: [
        containerTemplate(name: 'worker', image: 'purbon/kafka-topology-builder:latest', command: 'cat', ttyEnabled: true),
        )

node("p3-kafka-topology-builder"){
        stage ('Checkout') {
            checkout scm
            withCredentials([usernamePassword(credentialsId: 'confluent-cloud', usernameVariable: 'CLUSTER_API_KEY', passwordVariable: 'CLUSTER_API_SECRET')])

        stages {
              stage('run') {
                steps {
                       {
                        sh './demo/build-connection-file.sh > topology-builder.properties'
                       }
                      sh 'kafka-topology-builder.sh --clientConfig topology-builder.properties --topology ${TopologyFiles} --allowDelete'
                  }
              }