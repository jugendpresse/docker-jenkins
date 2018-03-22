node {
    def jenkins

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        jenkins = docker.build("jugendpresse/jenkins")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */

        withCredentials([usernamePassword( credentialsId: 'jpdtechnicaluser', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {

            docker.withRegistry('', 'jpdtechnicaluser') {
                sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                jenkins.push("lts")
            }
        }
    }
}
