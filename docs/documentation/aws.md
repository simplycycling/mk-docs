# AWS

### AWS cli

Commands that I have found useful:

Spin up CloudFormation stack, passing key name variable:

    aws cloudformation create-stack 
        --stack-name test-stack 
        --template-body file:///path/to/template 
        --parameters ParameterKey=KeyName,ParameterValue=key-name

Delete the stack:

    aws cloudformation delete-stack --stack-name test-stack
    
Validate the stack template:

    aws cloudformation validate-template --template-body file:////path//to//template
    
I have no idea why the slashes have to be doubled in that command, but it works.

How many total nodes are running:

    aws ec2 describe-instances | grep running | wc -l
    
This is a really good example of the kind of flexibility and power you 
have with the aws command line tools - it describes instances, broken down into
instance ID, running state, Availability Zone, and tags:

    aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, State.Name, Placement.AvailabilityZone, Tags]'
    
And this is just convenient, if you need to create a new key.

    aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
    
Most of these commands will give you carpal tunnel syndrome if you type
them out, regularly, so I'd recommend aliasing them.

The following is for when you decide to learn packer, and don't want to open the console to deregister the ami and delete the snapshot.

    aws ec2 --profile rog deregister-image --image-id ami-xxxxxxx
    aws ec2 --profile rog describe-snapshots --owner-ids xxxxxxxxxxxx
    aws ec2 --profile rog delete-snapshot --snapshot-id snap-xxxxxxx

For that second command, you're going to get the owner id from the profile page of the owner of your AWS account.