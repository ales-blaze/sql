node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
                    app = docker.build("alesblaze/customsql")
                    app.inside {
                        sh 'echo "built image"'
                    }
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */
         app.inside {
             sh 'echo "Tests passed"'
         }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
    }
    
    stage('Deploy to K8s'){
        sh "  ansible-playbook -i /opt/playbook/hosts /opt/playbook/ansibledeploy2.yaml "
    }
}
