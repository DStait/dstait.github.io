---
title: "Lambda as a container"
---

# Files

Example file structure:
hello-world
  |-- main.py
  |-- requirements.txt
  |-- Dockerfile



## Dockerfile
```dockerfile
##
## Build image
##

FROM public.ecr.aws/lambda/python:3.11 as build-image

# Copy function code
RUN mkdir -p ${LAMBDA_TASK_ROOT}
COPY . ${LAMBDA_TASK_ROOT}

# Set working directory to function root directory
WORKDIR ${LAMBDA_TASK_ROOT}


# Install the function's dependencies
RUN pip install \
    --target ${LAMBDA_TASK_ROOT} \ 
    -r requirements.txt


##
## Main image
##

FROM public.ecr.aws/lambda/python:3.11

# Include global arg in this stage of the build
ARG APP_DIR
# Set working directory to function root directory
WORKDIR ${LAMBDA_TASK_ROOT}

# Copy in the built dependencies
COPY --from=build-image ${LAMBDA_TASK_ROOT} ${LAMBDA_TASK_ROOT}

# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
CMD [ "main.handler" ]
```

## main.py
```python
import sys

def handler(event, context):
    return "Hello from AWS Lambda using Python" + sys.version + "!"
```


# Build, Run & Test
## Build
    docker build -t docker-image:test .

## Run
    docker run -p 9000:8080 docker-image:test

## Test
    curl "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'
