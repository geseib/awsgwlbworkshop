+++
title = "Deploy Infrastructure"
chapter = false
weight = 10
+++

## Launch Template

We will use a cloudformation template that builds out all of the components; creating a Transit Gateway, 2 VPCs, subnets, gateways, and two aEC2 instances. This type of Infrastructure as code can create consistent repeatable configurations, as well as provide a faster path to production.

### Prerequisites ###
You will need the following in the region you plan to deploy:
- **S3 bucket** 
  - Create bucket with a identifying name in deployment region

    {{% button href="https://s3.console.aws.amazon.com/s3/bucket/create?region=us-east-1" icon="fab fa-aws" icon-position="right" %}}Create S3 bucket(us-east-1){{% /button %}}
<br />
    {{% button href="https://s3.console.aws.amazon.com/s3/bucket/create?region=us-west-2" icon="fab fa-aws" icon-position="right" %}}Create S3 bucket(us-west-2){{% /button %}}
<br />
    {{% button href="https://s3.console.aws.amazon.com/s3/bucket/create?region=eu-west-1" icon="fab fa-aws" icon-position="right" %}}Create S3 bucket(eu-west-1){{% /button %}}
<br />
  - Copy these files locally to your computer (right click and choose Save Link As..."), and then upload them to S3 in folder config as such:
  *****my_bootstrap_bucket*****
      L **config**
          L **[bootstrap.xml](https://seib-paloalto-bootstrap.s3-eu-west-1.amazonaws.com/config/bootstrap.xml)**
          L **[init-cfg.txt](https://seib-paloalto-bootstrap.s3-eu-west-1.amazonaws.com/config/init-cfg.txt)**
    *Be sure to verify that the files are uploaded to the folder **config** and not the root of the bucket.*
    


- **EC2 key pair** in the region you plan to deploy

    {{% button href="https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#CreateKeyPair:" icon="fab fa-aws" icon-position="right" %}}Create key pair (us-east-1){{% /button %}}
<br />
    {{% button href="https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#CreateKeyPair:" icon="fab fa-aws" icon-position="right" %}}Create key pair(us-west-2){{% /button %}}
<br />
    {{% button href="https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#CreateKeyPair:" icon="fab fa-aws" icon-position="right" %}}Create key pair(eu-west-1){{% /button %}}
<br />

- **Subscibe to the {{< rawhtml >}}<a href="https://aws.amazon.com/marketplace/pp/B083LH64T3?ref_=srh_res_product_title" target="_blank">Palo Alto VMSeries appliance</a>{{< /rawhtml >}}

>(EC2 instances running this image may incur additional software cost per hour. However Palo Alto does offer a **free trial** *Try one unit of this product for 15 days. There will be no software charges for that unit, but AWS infrastructure charges still apply. Free Trials will automatically convert to a paid subscription upon expiration and you will be charged for additional usage above the free units provided.*)

#### Deploy Cloudformation Stack
1. Click on the CloudFormation Launch link below that corresponds to the AWS Region in which you want to deploy the workshop.

    {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=gwlblab&templateURL=https://gwlb-resourcesbucket-1ad6ln1ib92aa.s3.amazonaws.com/networkingdemos-gwlbworkshop.yml&param_VMSeriesAMI=ami-0847cff6598da0a2f" icon="fab fa-aws" icon-position="right" %}}Create CloudFormation stack (us-east-1){{% /button %}}
<br />
    {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=gwlblab&templateURL=https://gwlb-resourcesbucket-1ad6ln1ib92aa.s3.amazonaws.com/networkingdemos-gwlbworkshop.yml&param_VMSeriesAMI=ami-0f8c3e2c1b7b4bcc9" icon="fab fa-aws" icon-position="right" %}}Create CloudFormation (us-west-2){{% /button %}}
<br />
    {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=gwlblab&templateURL=https://gwlb-resourcesbucket-1ad6ln1ib92aa.s3.amazonaws.com/networkingdemos-gwlbworkshop.yml&param_VMSeriesAMI=ami-00ad2b17de74dd860" icon="fab fa-aws" icon-position="right" %}}Create CloudFormation (eu-west-1){{% /button %}}
<br />



1. Leave the parameters at their defaults, except the EC2 Key Pair (pick the key pair you just created) and the S3 bucket with the bootstrap files. Scroll down to the bottom of the **Quick create stack** screen and check the **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** Click the **Create** button in the lower right.

1) After about 4-8 minutes, your VPCs will be deployed with several EC2 Instances. (dont forget to remove these resources at the end of the lab.)

### You have completed the Launch VPCs using AWS Cloudformation.

