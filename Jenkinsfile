#!groovy

node {
    // stage('init') {
    //     // git clone
    //     checkout scm
    // }
    def dockerfile = 'Dockerfile'
    def repo_name = 'node-jenkins-demo'
    def imageName = 'mostuf556/node-jenkins-demo'
    def tag = 'latest' //1.0.${env.BUILD_ID}

    stage('prepare') {
        git url: "https://github.com/brownman/${repo_name}.git"
        sh 'ls -la'
    }

    // stage('SCM Checkout') {
    //     git credentialsId: 'git-creds', url: 'https://github.com/brownman/node-hello.git'
    // }

    stage('Build Docker Image and run test') {
            // sh 'docker build -t mosut/my-app:2.0.0 .'
            // customImage = docker.build("${imageName}:${env.BUILD_ID}", "-f ${dockerfile} .")

            def testImage = docker.build("${imageName}",  "-f ${dockerfile} .")

        testImage.inside {
            sh 'npm install'

            sh 'npm test'
        }
    }

        // stage('Push to docker hub (branch: master)') {
        //     // when {
        //     //     // skip this stage unless branch is NOT master
        //     //     branch 'master'
        //     // }
        //     environment {
        //         DOCKERHUB_CREDENTIALS = credentials('docker_hub_access_token')
        //     }

        // sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        //         sh 'docker push $DOCKERHUB_CREDENTIALS_USR/$repo_name:$tag'
        // //    sh 'docker logout'
        // sh 'docker logout'
        // //  post('logout') {
        // //     always {
        // //             sh 'docker logout'
        // //     }
        // // }
        // } //stage

    stage('Push Docker Image') {

                if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'


                    withCredentials([usernamePassword(credentialsId: 'docker_hub_access_token', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                //   sh 'echo $USERNAME -p $PASSWORD > /tmp/password'
                // sh "docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PWD}"
                sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
        }

//         withCredentials([string(credentialsId: 'docker_hub_access_token', variable: 'DOCKERHUB_CREDENTIALS')]) { //
//             sh 'env > /tmp/env2'

        //   }
        sh "docker push ${imageName}:${tag}"

        } else {
            echo 'I execute elsewhere'
        }




    // sh "docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PWD}"
    // sh "docker login -u ${dockerHubUsr} -p ${dockerHubPwd}"
    }

    // docker.withRegistry('https://docker.hub.com', 'docker_hub_access_token') {
    //     def customImage = docker.build("${imageName}:${env.BUILD_ID}")

//     /* Push the container to the custom Registry */
//     customImage.push()
// }
}

//where to run ?
    // docker.withServer('tcp://swarm.example.com:2376', 'swarm-certs') {
    //     docker.image('mysql:5').withRun('-p 3306:3306') {
    //         /* do things */
    //     }
    // }
