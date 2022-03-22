
Create IAM policies
1.1 Create policy named ec2read
This policy allows read only permissions with a region condition. 
    Sign in to the AWS Management Console as an IAM user with MFA enabled that can assume roles in your AWS account, and open the IAM console at https://console.aws.amazon.com/iam/ . 
   In the navigation pane, click Policies and then click Create policy.

 
    On the Create policy page click the JSON tab.
 


    Replace the example start of the policy that is already in the editor with the policy below.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ec2read",
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*",
                "ec2:Get*"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": [
                        "us-east-1",
                        "us-west-1"
                    ]
                }
            }
        }
    ]
}
    Click Review policy.
    Enter the name of ec2read and any description to help you identify the policy, verify the summary and then click Create policy.
 

 

1.2 Create policy named ec2tags
This policy allows the creation of tags for EC2, with a condition of the action being RunInstances , which is launching an instance.
    Create a managed policy using the JSON policy below and name of ec2tags.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ec2tags",
            "Effect": "Allow",
            "Action": "ec2:CreateTags",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:CreateAction": "RunInstances"
                }
            }
        }
    ]
}

 
 
 



1.3 Create policy named ec2existingtagscreate
This policy allows creation (and overwriting) of EC2 tags only if the resources are already tagged Team / Beta.
    Create a managed policy using the JSON policy below and name of ec2existingtagscreate.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ec2existingtagscreate",
            "Effect": "Allow",
            "Action": "ec2:CreateTags",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Team": "Beta"
                },
                "ForAllValues:StringEquals": {
                    "aws:TagKeys": [
                        "Team",
                        "Name"
                    ]
                },
                "StringEqualsIfExists": {
                    "aws:RequestTag/Team": "Beta"
                }
            }
        }
    ]
}
 
 


1.4 Create policy named ec2instances
This first section of this policy allows instances to be launched, only if the conditions of region and specific tag keys are matched. The second section allows other resources to be created at instance launch time with region condition.
    Create a managed policy using the JSON policy below and name of ec2instances.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ec2instances",
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "arn:aws:ec2:*:*:instance/*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": [
                        "us-east-1",
                        "us-west-1"
                    ],
                    "aws:RequestTag/Team": "Beta"
                },
                "ForAllValues:StringEquals": {
                    "aws:TagKeys": [
                        "Name",
                        "Team"
                    ]
                }
            }
        },
        {
            "Sid": "ec2instancesother",
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": [
                "arn:aws:ec2:*:*:subnet/*",
                "arn:aws:ec2:*:*:key-pair/*",
                "arn:aws:ec2:*::snapshot/*",
                "arn:aws:ec2:*:*:launch-template/*",
                "arn:aws:ec2:*:*:volume/*",
                "arn:aws:ec2:*:*:security-group/*",
                "arn:aws:ec2:*:*:placement-group/*",
                "arn:aws:ec2:*:*:network-interface/*",
                "arn:aws:ec2:*::image/*"
            ],
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": [
                        "us-east-1",
                        "us-west-1"
                    ]
                }
            }
        }
    ]
}
 
 

1.5 Create policy named ec2instancesmanage
This policy allows reboot, terminate, start and stop of instances, with a condition of the key Team is Beta and region.
    Create a managed policy using the JSON policy below and name of ec2instancesmanage.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ec2instancesmanage",
            "Effect": "Allow",
            "Action": [
                "ec2:RebootInstances",
                "ec2:TerminateInstances",
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Team": "Beta",
                    "aws:RequestedRegion": [
                        "us-east-1",
                        "us-west-1"
                    ]
                }
            }
        }
    ]
}

 
 
 




    

