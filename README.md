# Infastructure as Code: Introducing AWS CloudFormation

AWS CloudFormation presentation and examples, for the Northern California Oracle User Group

Paul Marcelin

![Bilby Stampede](/image/mail-paul.png)

August, 2017

## Warnings

* The resources in the CloudFormation templates are not free. **You will incur [AWS charges](https://aws.amazon.com/pricing/services/)** from the moment you create the stacks to the moment that you delete the stacks as well as any resources not set for automatic deletion. You are responsible for any AWS charges.

* The CloudFormation templates are provided as-is. They are **not immediately suitable for production**. Before you use them, you are responsible for modifying the templates to meet your architectural, performance and security needs.

## Instructions

1. Clone this repository, or save the template files in the [/example](/example) directory to your local system. The templates are text files.

2. Create an [AWS](https://aws.amazon.com/) account, if you have not already done so.

3. Log in to the AWS Web Console. To find the link, which is specific to your account, see ["The IAM Console and the Sign-in Page"](http://docs.aws.amazon.com/IAM/latest/UserGuide/console.html).
   
   Logging in as an IAM (Identity and Access Management) user, rather than as the root user, is always recommended.
   
4. Follow the instructions in the [presentation](/presentation/aws-cloudformation-nocoug-marcelin.pdf) to create CloudFormation stacks.

   You will upload the template files from your local system, one at a time, in numeric order. Name the stacks as you wish. Accept the default parameter values, except that you must manually select your default Virtual Private Cloud (VPC), select any two subnets, and enter a master database user password, when prompted for those parameters.
   
   Before creating the final stack, check the [CloudFormation Console](https://console.aws.amazon.com/cloudformation/). Make sure that the  Status of all prior stacks is `CREATE_COMPLETE`. Creating this stack, which contains the Relational Database Service (RDS) instance, may take as long as 1 hour.

5. Create a change set for the database instance stack, to increase the storage from 20 to 25 GiB, as explained in the presentation. Executing this change set may take as long as 1 hour.

6. Decide whether you wish to connect to your Oracle database from one of your existing AWS Elastic Compute Cloud (EC2) instances, or from your local system.

   a. EC2 Instance
   
      The EC2 instance must be in the same VPC as the RDS instance. Note the security group identifier exported by the _InVPC_ security group pair stack as `SecGrp-DefaultVPC-OracleDB-Client-App-InVpc-Id`. In the [EC2 Console](https://console.aws.amazon.com/ec2/), add the security group to your EC2 instance.
   
   b. Local system
   
      Confirm the current [public IP address](https://whatismyipaddress.com/) of your local system. Create a change set for the _ExVpc_ security group stack, replacing `127.0.0.1` in the _ingress_ rule with your public IP address.

7. Test network connectivity.

   Note the database endpoint and port, which are exported by the database instance stack as RdsInst-_StackName_-Endpoint and RdsInst-_StackName_-Port, where _StackName_ is the name you assigned to the database instance stack.

   Telnet is included by default with Linux and MacOS. Follow these instructions to [enable Telnet in Windows](http://m.wikihow.com/Activate-Telnet-in-Windows-7) (the instructions mention Windows 7, but will work in any recent version of Windows).
   
   On your EC2 instance or your local system, run `telnet `_`endpoint`_` `_`port`_, where _endpoint_ and _port_ are the values that you have noted.
   
   If the connection fails, Telnet will eventually exit. If the connection succeeds, Telnet will wait for activity. In that case, hold down the Control key and press the ']' key; this is the standard "escape" code. Then, type `quit`.

8. Optionally, connect to the database from your EC2 instance or your local system.

   Install suitable Oracle client software in the manner appropriate for your operating system. 

   Connect using the endpoint, port, and master user name (`master`), and the master user password that you entered when you created the database instance stack.

9. Delete the database stack. Then, in the [RDS Console](https://console.aws.amazon.com/rds/), **delete the sample database instance without saving a final snapshot**.

10. Delete the remaining sample CloudFormation stacks.

## Feedback

I appreciate corrections, comments and suggestions. Pull requests are most welcome. Thank you!
