node {
    def app

    stage('Cloning the repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build Docker Image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("luislink24/webpage")
    }

    stage('Test') {
        /* Ideally, we would run a test framework against our image.
         * Just an example */

        app.inside("--entrypoint=''") {
                          echo "Test con exito!"
       }
    }

    stage('Push Docker Image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage ('Clean') {
    deleteDir()
    }

    stage ('Successfully completed') {
    checkout scm 
    }
    
    stage ('Send Slack') {
    success {
                slackSend "Build deployed successfully - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            }
    }
    
}


