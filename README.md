

Create 5 IAM policies:

1. Create policy named ec2read
This policy allows read only permissions with a region condition. 
   
2. Create policy named ec2tags
This policy allows the creation of tags for EC2, with a condition of the action being RunInstances , which is launching an instance.
    
3. Create policy named ec2existingtagscreate
This policy allows creation (and overwriting) of EC2 tags only if the resources are already tagged Team / Beta.
    
4. Create policy named ec2instances
This first section of this policy allows instances to be launched, only if the conditions of region and specific tag keys are matched. The second section allows other resources to be created at instance launch time with region condition.
    
5. Create policy named ec2instancesmanage
This policy allows reboot, terminate, start and stop of instances, with a condition of the key Team is Beta and region.
