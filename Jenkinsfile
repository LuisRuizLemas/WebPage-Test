node {
    def app

    stage('Clonando el repositorio') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Haciendo Build a la imagen') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("luislink24/webpage")
    }

    stage('Realizando Pruebas a la imagen') {
        /* Ideally, we would run a test framework against our image.
         * Just an example */

        app.inside("--entrypoint=''") {
                          echo "Test con exito!"
       }
    }

    stage('Subiendo imagen') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage ('Limpiando') {
    deleteDir()
    }

    stage ('Checkout') {
    checkout scm 
    }

    }
}
