pipeline {
 environment {
        region = "us-east-1"
        docker_repo_uri = "public.ecr.aws/u6n4i6i0/sample-app"
        task_def_arn = "arn:aws:ecs:us-east-1:547850580937:task-definition/first-run-task-definition"
        cluster = "default"
        exec_role_arn = "arn:aws:iam::547850580937:role/jenkins"
}
stage('Deploy') {
    steps {
        // Override image field in taskdef file
        sh "sed -i 's|{{image}}|${docker_repo_uri}:${commit_id}|' taskdef.json"
        // Create a new task definition revision
        sh "aws ecs register-task-definition --execution-role-arn ${exec_role_arn} --cli-input-json file://taskdef.json --region ${region}"
        // Update service on Fargate
        sh "aws ecs update-service --cluster ${cluster} --service sample-app-service --task-definition ${task_def_arn} --region ${region}"
    }
}
