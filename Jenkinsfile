node {
    def application = "pythonapp"
    def dockerhubaccountid = "anand1104"
	def BUILD_NUMBER = 1.0
    stage('Clone repository') {
        steps {
                script {
                    // Clone the Git repository's master branch
                    def gitRepoUrl = 'https://github.com/AnandPG/Jenkins_Upgradev3.git'

                    checkout([$class: 'GitSCM', 
                        branches: [[name: '*/master']], 
                        userRemoteConfigs: [[url: gitRepoUrl]], 
                        extensions: [[$class: 'CleanBeforeCheckout'], [$class: 'CloneOption', noTags: false, shallow: true, depth: 1]]
                    ])
                }
            }
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([ credentialsId: "26bf06be-57ef-4f75-bc44-c1a362b8b896", url: "" ]) {
        app.push()
        app.push("latest")
    }
    }

    stage('Deploy') {
        sh ("docker run -d -p 3333:3333 ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Remove old images') {
        // remove old docker images
        sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}