---
title: "Lambda container deployment" 
---


```terraform
provider "aws" {
    region = "eu-west-2"
    profile = "sandbox"
}

variable "region" {
    type = string
    default = "eu-west-2"
}

data aws_caller_identity current {}

locals {
 account_id          = data.aws_caller_identity.current.account_id
 ecr_repository_name = "demo-hello-world"
 ecr_image_tag       = "latest"
}

resource aws_ecr_repository repo {
 name = local.ecr_repository_name
}

resource null_resource ecr_image {
 triggers = {
   python_file = md5(file("${path.module}/lambda/hello-world/main.py"))
   docker_file = md5(file("${path.module}/lambda/hello-world/Dockerfile"))
 }
 
 provisioner "local-exec" {
   command = <<EOF
           aws ecr get-login-password --region ${var.region} --profile sandbox | docker login --username AWS --password-stdin ${local.account_id}.dkr.ecr.${var.region}.amazonaws.com
           cd ${path.module}/lambda/hello-world
           docker buildx build --platform linux/amd64 -t ${aws_ecr_repository.repo.repository_url}:${local.ecr_image_tag} .
           docker push ${aws_ecr_repository.repo.repository_url}:${local.ecr_image_tag}
       EOF
 }
}

data aws_ecr_image "lambda_image" {
 depends_on = [
   null_resource.ecr_image
 ]
 repository_name = local.ecr_repository_name
 image_tag       = local.ecr_image_tag
}


resource aws_iam_role "lambda" {
 name = "demo-hello-world-lambda-role"
 assume_role_policy = <<EOF
{
   "Version": "2012-10-17",
   "Statement": [
       {
           "Action": "sts:AssumeRole",
           "Principal": {
               "Service": "lambda.amazonaws.com"
           },
           "Effect": "Allow"
       }
   ]
}
 EOF
}

resource aws_lambda_function "hello-world" {
 depends_on = [
   null_resource.ecr_image
 ]
 function_name = "demo-hello-world-lambda"
 role = aws_iam_role.lambda.arn
 timeout = 300
 image_uri = "${aws_ecr_repository.repo.repository_url}@${data.aws_ecr_image.lambda_image.id}"
 package_type = "Image"
}
```
