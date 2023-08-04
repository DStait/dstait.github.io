---
title: "AWS Lambda Development (VSCode)"
---

### PreReqs

AWS SAM CLI installed - https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html

AWS Toolkit installed in VSCode - https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode

## Create a launch.json
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "aws-sam",
            "request": "direct-invoke",
            "name": "Invoke Python AWS Lambda",
            "invokeTarget": {
                "target": "code",
                "lambdaHandler": "main.lambda_handler",
                "projectRoot": "${workspaceFolder}/lambda/"
            },
            "lambda": {
                "runtime": "python3.8",
                "timeoutSec": 10,
                "payload": {
                    "path": "event.json"
                }
            },
            "aws": {
                "credentials": "profile:sandbox"
            }
        }
    ]
}
```

## Create the event.json
Create a json file in the root of the workspace with the content of the event to send to the lambda 