# AWS_ECS
use ECS to create an web server on container. Using Load Balancers.
# Create Target Groups
![image](https://user-images.githubusercontent.com/75282285/225976988-8d08a420-5238-4cd9-9bce-6a877c97c2a7.png)

# Create Load Balancers

![image](https://user-images.githubusercontent.com/75282285/225977183-095a186b-e86f-4293-8649-42f7e1f6d7ec.png)

![image](https://user-images.githubusercontent.com/75282285/225977110-8c7f82bd-60c6-493c-9008-7cb38be48ee7.png)

# Create Security Group
![image](https://user-images.githubusercontent.com/75282285/225977266-270a11bf-4fd3-4aeb-8876-c99a451eae14.png)

# Create role
![image](https://user-images.githubusercontent.com/75282285/225977331-485b14f4-8d49-45e4-93e0-7598ded9566e.png)

# Create cluster
![image](https://user-images.githubusercontent.com/75282285/225977401-f7dcd604-959f-4c13-ab4a-5c276d422091.png)

# Create Task
![image](https://user-images.githubusercontent.com/75282285/225977509-0289e706-f7b4-43f9-a7e5-041aea70bd80.png)

```
{
    "family": "tstest-fargate-task",
    "executionRoleArn": "arn:aws:iam::774336841216:role/zsbfullroles",
    "networkMode": "awsvpc",
    "containerDefinitions": [
        {
            "name": "fargate-app1",
            "command": [
                "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>ZSB ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
            ],
            "entryPoint": ["sh","-c"],
            "image": "httpd:2.4",
            "portMappings": [
                {
                    "containerPort": 80,
                    "hostPort": 80,
                    "protocol": "tcp"
                }
            ],
            "essential": true
        }
    ],
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "256",
    "memory": "512"
}

```

![image](https://user-images.githubusercontent.com/75282285/225977659-2238f5e0-3fe5-491e-aee4-b4f550a4ffb4.png)


# Create service
![image](https://user-images.githubusercontent.com/75282285/225977738-a111f03c-1f53-4147-8b0a-b627380a3dff.png)


![image](https://user-images.githubusercontent.com/75282285/225977786-bfa58bf0-5a5e-41ed-92c5-fcfdd7e8974c.png)


![image](https://user-images.githubusercontent.com/75282285/225977820-c73156c1-3132-4b7e-a2ee-4ceeb29debc5.png)


![image](https://user-images.githubusercontent.com/75282285/225977872-3aca1a35-28d0-4940-8d98-2e44063129ce.png)


![image](https://user-images.githubusercontent.com/75282285/225977889-7112e5f8-51f1-4993-afbb-76a4b6bf1b31.png)

Also can use JSON to create the service on ECS.
```
{
    "cluster": "mycluster",
    "serviceName": "tstest-svc",
    "taskDefinition": "arn:aws:ecs:us-east-2:774336841216:task-definition/tstest-fargate-task:1",
    "loadBalancers": [
        {
            "targetGroupArn": "arn:aws:elasticloadbalancing:us-east-2:774336841216:targetgroup/zsbtargetgroup2/1214af2abe6cf3be",
            "containerName": "fargate-app1",
            "containerPort": 80
        }
    ],
    "desiredCount": 2,
    "launchType": "FARGATE",
    "platformVersion": "1.4.0",
    "deploymentConfiguration": {
        "maximumPercent": 200,
        "minimumHealthyPercent": 100
    },
    "networkConfiguration": {
        "awsvpcConfiguration": {
             "subnets": [
                  "subnet-0480e327387f01528"
             ],
             "securityGroups": [
                  "sg-0d87b011775d3275a"
             ],
             "assignPublicIp": "ENABLED"		
        }
    },
    "healthCheckGracePeriodSeconds": 0,
    "schedulingStrategy": "REPLICA"
}

```


