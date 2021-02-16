+++
title = "Deploy Infrastructure"
chapter = false
weight = 10
+++

## Launch Template

We will use a cloudfromation template that builds out all of the components; creating a Trasnit Gateway, 2 VPCs, subnets, gateways, and two aEC2 instances. This type of Infrastrucutre as code can create consistent repeatable configurations, as well as provide a faster path to production.

### Prerequisites ###
You will need the following in the region you plan to deploy:
- **S3 bucket** 
  - Create bucket with a identifying name in deployment region
  {{< rawhtml >}}
  <ul><li><a href="https://s3.console.aws.amazon.com/s3/bucket/create?region=us-east-1" target="_blank">us-east-1 bucket</a></li>
 <li><a href="https://s3.console.aws.amazon.com/s3/bucket/create?region=us-west-2" target="_blank">us-west-2 bucket</a></li>
 <li><a href="https://s3.console.aws.amazon.com/s3/bucket/create?region=eu-west-1" target="_blank">eu-west-1 bucket</a>
  </li></ul>
{{< /rawhtml >}}
  - Copy these files in folder config (right click and choose Save Link As...")
  my_bootstrap_bucket
    L config
      L__ [bootstrap.xml](https://seib-paloalto-bootstrap.s3-eu-west-1.amazonaws.com/config/bootstrap.xml)
      L__ [init-cfg.txt](https://seib-paloalto-bootstrap.s3-eu-west-1.amazonaws.com/config/init-cfg.txt)

- **EC2 key pair** in the region you plan to deploy
  {{< rawhtml >}}
  <ul><li><a href="https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#CreateKeyPair:" target="_blank">us-east-1 key pair console</a></li>
 <li><a href="https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#CreateKeyPair:" target="_blank">us-west-2 key pair console</a></li>
 <li><a href="https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#CreateKeyPair:" target="_blank">eu-west-1 key pair console</a>
  </li></ul>
{{< /rawhtml >}}

- **Subscibe to the {{< rawhtml >}}<a href="https://aws.amazon.com/marketplace/pp/B083LH64T3?ref_=srh_res_product_title" target="_blank">Palo Alto VMSeries appliance</a>{{< /rawhtml >}}

>(EC2 instances running this image may incur additional software cost per hour. However Palo Alto does offer a **free trial** *Try one unit of this product for 15 days. There will be no software charges for that unit, but AWS infrastructure charges still apply. Free Trials will automatically convert to a paid subscription upon expiration and you will be charged for additional usage above the free units provided.*)

1. Click on the CloudFormation Launch link below that corresponds to the AWS Region in which you want to deploy the workshop.

   [![US East (N.Va.)](https://samdengler.github.io/cloudformation-launch-stack-button-svg/images/us-east-1.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=gwlblab&templateURL=https://{{<codebucket>}}.s3.amazonaws.com/networkingdemos-gwlbworkshop.yml&param_VMSeriesAMI=ami-0847cff6598da0a2f)
   [![US West (Oregon)](https://samdengler.github.io/cloudformation-launch-stack-button-svg/images/us-west-2.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=gwlblab&templateURL=https://{{<codebucket>}}.s3.amazonaws.com/networkingdemos-gwlbworkshop.yml&param_VMSeriesAMI=ami-0f8c3e2c1b7b4bcc9)
   [![EU West (Ireland)](https://samdengler.github.io/cloudformation-launch-stack-button-svg/images/eu-west-1.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=gwlblab&templateURL=https://{{<codebucket>}}.s3.amazonaws.com/networkingdemos-gwlbworkshop.yml&param_VMSeriesAMI=ami-00ad2b17de74dd860)


1. Leave the parameters at their defaults, except the EC2 Key Pair (pick the key pair you just created) and the S3 bucket with the bootstrap files. Scroll down to the bottom of the **Quick create stack** screen and check the **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** Click the **Create** button in the lower right.

1) After about 4-8 minutes, your VPCs will be deployed with several EC2 Instances. (dont forget to remove these resources at the end of the lab.)

### You have completed the Launch VPCs using AWS Cloudformation.

