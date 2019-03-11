node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("xtime/spin-kub-v2-demo")
    }

     /* Ideally, we would run a test framework against our image.
    stage('Test image') {
       
       

        app.inside {
            sh 'echo "Tests passed"'
        }
    }
      * For this example, we're using a Volkswagen-type approach ;-) */

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
          * Pushing multiple tags is cheap, as all the layers are reused. */
        
        sh("eval \$(aws --region us-west-1 ecr get-login --no-include-email)")
        
        docker.withRegistry('https://498040579653.dkr.ecr.us-west-1.amazonaws.com/xtime/spin-kub-v2-demo') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
        
    }
}
