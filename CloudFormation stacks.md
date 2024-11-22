We have 2 stack: Network , webapp

The network stack is responsible for creating the network portion of the application

VPC, GATEWAYS & EIP section, SUBNETS, ROUTING, SECURITY GROUPS

[webapp.yaml](https://files.nuclino.com/files/f57c8e17-30e9-461f-9118-2bd7e65a1299/webapp.yaml)

[NetworkStack.yml](https://files.nuclino.com/files/66abf180-190f-449d-8951-e460bc410b12/NetworkStack.yml)

**Process for bringing environment up:**&#x20;

You can run the stack from commandline by using the below commands from CLI, just please make sure to Download the cloudformation yml files into your local laptop

# Commandline to create R53Rwebapp stack

Download cloudformation yml templates int your local laptop then run the following commands

$ aws cloudformation **create-stack** --stack-name **R53Rnetwork** --template-body file://NetworkStack.yml --region us-east-1 --output table

### Wait for the stack to be fully created

$aws cloudformation **wait** stack-create-complete --stack-name **R53Rnetwork** --region us-east-1

**Commandline to create R53Rwebapp stack:**&#x20;

$aws cloudformation **create-stack** --stack-name **R53Rwebapp** --template-body file://webapp.yaml --capabilities CAPABILITY\_NAMED\_IAM --region us-east-1 --output table

### Wait for the stack to be fully created

$aws cloudformation **wait** stack-create-complete --stack-name **R53Rwebapp** --region us-east-1

<br>

## #####     DELETING STACKS    ####\#

$aws cloudformation **delete-stack** --stack-name **R53Rwebapp** --region us-east-1

$aws cloudformation **delete-stack** --stack-name **R53Rnetwork** --region us-east-1

<br>
