# AWS CLI Operations

This guide provides step-by-step instructions for performing basic operations using the AWS Command Line Interface (CLI) to manage resources in Amazon Web Services (AWS).

## Main Points

- **Creating a new EC2 instance**
- **Creating a new security group**
- **Creating a new SSH key pair**
- **Setting up IAM users and groups**
- **Assigning permissions to IAM users**
- **Creating access keys for IAM users**

## Creating a New EC2 Instance

 **Get VPC ID:**
   
   ```sh
   aws ec2 describe-vpcs

   ```
1- **Create New Security Group:**

```sh
aws ec2 create-security-group --group-name my-sg --description "My SG" --vpc-id VPC-ID
```

<img src="https://i.imgur.com/tFx1ms7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

2- **Allow SSH Access (Port 22) in Security Group:**

```sh
aws ec2 authorize-security-group-ingress --group-id SECURITY-GROUP-ID --protocol tcp --port 22 --cidr 82.42.114.199/32
```

<img src="https://i.imgur.com/YU733EZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

<img src="https://i.imgur.com/UerAA8z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

3- **Create New SSH Key Pair:**

```sh
aws ec2 create-key-pair --key-name MyKpCli --query 'KeyMaterial' --output text > MyKpCli.pem
```
<img src="https://i.imgur.com/f5pNaWc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

-4 **Describe Subnets:**

```sh
aws ec2 describe-subnets

```

****

-5 **Run EC2 Instance:**

```sh

aws ec2 run-instances --image-id AMI-ID --count 1 --instance-type t2.micro --key-name MyKpCli --security-group-ids SECURITY-GROUP-ID --subnet-id SUBNET-ID

```
<img src="https://i.imgur.com/QbSQyVN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

-6 **SSH into EC2 Instance:**

```sh

ssh -i MyKpCli.pem ec2-user@INSTANCE-PUBLIC-IP

```

<img src="https://i.imgur.com/zBTG7Co.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

<img src="https://i.imgur.com/YIAiayG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

## IAM Operations

1-**Create User and Group:**

```sh
aws iam create-group --group-name MyGroupCli
aws iam create-user --user-name MyUserCli
aws iam add-user-to-group --user-name MyUserCli --group-name MyGroupCli

```

<img src="https://i.imgur.com/PLkBVbO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

2-**Assign Permissions for EC2 Service:**

```sh

aws iam attach-group-policy --group-name MyGroupCli --policy-arn POLICY-ARN

```

<img src="https://i.imgur.com/OyLCKtG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

3-**Create Credentials for New User:**

```sh

aws iam create-login-profile --user-name MyUserCli --password MyPassword! --password-reset-required

```

<img src="https://i.imgur.com/y90LYMc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

-4**Create and Assign Policy for Changing Password:**

- Create a JSON file named changePwdPolicy.json with the specified content.

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:ChangePassword"
            ],
            "Resource": [
                "arn:aws:iam::211125390603:user/MyUserCli"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetAccountPasswordPolicy"
            ],
            "Resource": "*"
        }
    ]
}

```

- Run the following commands to create and attach the policy:

```sh

aws iam create-policy --policy-name changepwd --policy-document file://changePwdPolicy.json
aws iam attach-group-policy --group-name MyGroupCli --policy-arn arn:aws:iam::ACCOUNT-ID:policy/changepwd

```

<img src="https://i.imgur.com/k5xQvLe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

-5**Create Access Key for User:**

```sh

aws iam create-access-key --user-name MyUserCli

```
<img src="https://i.imgur.com/VYaSUeU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****


-6**Switch AWS Users for AWS CLI Commands:**

```sh

export AWS_ACCESS_KEY_ID=ACCESS-KEY-ID
export AWS_SECRET_ACCESS_KEY=SECRET-ACCESS-KEY

```
