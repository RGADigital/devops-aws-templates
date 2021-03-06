# EC2 Container Service Template (with VPC Parameters in template)

## Description
This Cloudformation template sets up a distributed EC2 Container Service (ECS) stack. This includes an auto scaling group, security groups, and an application load balancer.

## Parameters File

The default-params.json file is a sample parameters file with stub values. You should replace these values and run the aws cli to trigger the building of your stack.

## Launch Cloudformation Stack via AWS CLI

The following aws cli command will build the stack using the params listed in the parameters file. This command must be run from the location of the template and parameters file.  

Set up these variables which will determine the stack name and tags:  
```
export AWS_REGION=us-east-1     \
AWS_PROFILE={profile_name}      \
STACK_NAME={stack_name}         \
OWNER_EMAIL={email_address}     \
ENV={environment}               \
PROJECT_NAME={project_name}     \
CLIENT_NAME={client_name}    
```

Run the aws cli create-stack command to trigger building your stack:  
```
aws cloudformation create-stack                     \
--stack-name ${STACK_NAME}-$(date +%Y%m%d%H%M%S)    \
--template-body file://$(pwd | tr -d '\n')/ecs-svc-tasks-asg-elb-cfn.yaml \
--capabilities CAPABILITY_NAMED_IAM                 \
--tags Key=Name,Value=${STACK_NAME}                 \
Key=owner,Value=${OWNER_EMAIL}                      \
Key=environment,Value=${ENV}                        \
Key=project,Value=${PROJECT_NAME}                   \
Key=client,Value=${CLIENT_NAME}                     \
--parameters file://$(pwd | tr -d '\n')/default-params.json \
--region ${AWS_REGION}      \
--profile ${AWS_PROFILE}
```

The --region argument must be passed in if one is not set in your aws profile.  
The --profile argument can be left out if you are using the default profile.  
  
