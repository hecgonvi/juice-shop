pipeline {
 //   ...

    environment {
        region = "us-east-1"
        docker_repo_uri = "public.ecr.aws/u6n4i6i0/sample-app"
        task_def_arn = "arn:aws:ecs:us-east-1:547850580937:task-definition/first-run-task-definition"
        cluster = "default"
        exec_role_arn = "arn:aws:iam::547850580937:role/jenkins"
    }

 //   ...
}
stage('Build') {
    steps {
        // Get SHA1 of current commit
        script {
            commit_id = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
        }
        // Build the Docker image
        sh "docker build -t ${docker_repo_uri}:${commit_id} ."
        // Get Docker login credentials for ECR
        sh "aws ecr get-login --no-include-email --region ${region} | sh"
        // Push Docker image
        sh "docker push ${docker_repo_uri}:${commit_id}"
        // Clean up
        sh "docker rmi -f ${docker_repo_uri}:${commit_id}"
    }
}
