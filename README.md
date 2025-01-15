Deploying a Flask API

This is the project starter repo for the course Server Deployment, Containerization, and Testing.

In this project, you will containerize and deploy a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild.

The Flask app that will be used for this project consists of a simple API with three endpoints:

GET '/': This is a simple health check, which returns the response 'Healthy'.

POST '/auth': This takes an email and password as JSON arguments and returns a JWT based on a custom secret.

GET '/contents': This requires a valid JWT and returns the unencrypted contents of that token.

The app relies on a secret set as the environment variable JWT_SECRET to produce a JWT. The built-in Flask server is adequate for local development, but not production, so you will be using the production-ready Gunicorn server when deploying the app.

ELB Endpoint URL

The application is deployed on an AWS Elastic Load Balancer (ELB). You can test the API using the following URL:

http://a702712ee21de4f4bb268b934150f088-1552340412.us-east-1.elb.amazonaws.com

Testing Endpoints

Health Check

curl --request GET 'http://a702712ee21de4f4bb268b934150f088-1552340412.us-east-1.elb.amazonaws.com/'

Response: "Healthy"

Generate JWT

export TOKEN=$(curl --data '{"email":"test@example.com","password":"password"}' \
    --header "Content-Type: application/json" \
    -X POST http://a702712ee21de4f4bb268b934150f088-1552340412.us-east-1.elb.amazonaws.com/auth | jq -r '.token')
echo $TOKEN

Decrypt JWT

curl --request GET 'http://a702712ee21de4f4bb268b934150f088-1552340412.us-east-1.elb.amazonaws.com/contents' \
    -H "Authorization: Bearer $TOKEN" | jq

Prerequisites

Docker Desktop - Installation instructions for all OSes can be found here.

Git: Download and install Git for your system.

Code editor: You can download and install VS code here.

AWS Account

Python version between 3.7 and 3.9. Check the current version using:

#  Mac/Linux/Windows
python --version

You can download a specific release version from here.

Python package manager - PIP 19.x or higher. PIP is already installed in Python 3 >=3.4 downloaded from python.org. However, you can upgrade to a specific version, say 20.2.3, using the command:

#  Mac/Linux/Windows Check the current version
pip --version
# Mac/Linux
pip install --upgrade pip==20.2.3
# Windows
python -m pip install --upgrade pip==20.2.3

Terminal

Mac/Linux users can use the default terminal.

Windows users can use either the GitBash terminal or WSL.

Command line utilities:

AWS CLI installed and configured using the aws configure command. Another important configuration is the region. Do not use the us-east-1 because the cluster creation may fail mostly in us-east-1. Let's change the default region to:

aws configure set region us-east-2  

Ensure to create all your resources in a single region.

EKSCTL installed in your system. Follow the instructions available here or here to download and install eksctl utility.

The KUBECTL installed in your system. Installation instructions for kubectl can be found here.

Initial setup

Fork the Server and Deployment Containerization Github repo to your Github account.

Locally clone your forked version to begin working on the project.

git clone https://github.com/SudKul/cd0157-Server-Deployment-and-Containerization.git
cd cd0157-Server-Deployment-and-Containerization/

These are the files relevant for the current project:

.
├── Dockerfile
├── README.md
├── aws-auth-patch.yml #ToDo
├── buildspec.yml      #ToDo
├── ci-cd-codepipeline.cfn.yml #ToDo
├── iam-role-policy.json  #ToDo
├── main.py
├── requirements.txt
├── simple_jwt_api.yml
├── test_main.py  #ToDo
└── trust.json     #ToDo

Project Steps

Completing the project involves several steps:

Write a Dockerfile for a simple Flask API

Build and test the container locally

Create an EKS cluster

Store a secret using AWS Parameter Store

Create a CodePipeline pipeline triggered by GitHub check-ins

Create a CodeBuild stage that will build, test, and deploy your code

For more detail about each of these steps, see the project lesson.

