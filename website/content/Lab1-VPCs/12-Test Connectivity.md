+++
title = "Test Connectivity"
chapter = false
weight = 12
+++

## Test from new EC2 Instance to the internet

It will take a few minutes for the Cloudformation template to to finish launching all of the resources.

1. From the **Amazon EC2** console and from the left menu select **Instances**.

   ![connect button](/images/gwlb-instances.png)

1. Check the box next to the new instance in the list (named VPC1-Server) and click the **Connect** button above the list.

   ![connect session manager](/images/gwlb-ssm-connect.png)

1. Click the tab **Session Manager** and click the **Connect** button. _this will bring up a new browser tab with the Linux bash shell_

   ![curl VPC2 Server](/images/gwlb-curlvpc1server.png)

1. At the **sh-4.2\$** prompt, curl **10.2.2.10**.

   ```
   curl 10.2.2.10
   ```

   This will return something like the following

   ```
   <html>
   <body>
   Welcome to my VPC1 web server. Server private IP is 10.1.2.10
   Availability Zone: us-west-2a
   Stack Name: gwlblab
   your ip is 127.0.0.1
   <h2>Headers</h2>
   Host: localhost<br>User-Agent: curl/7.61.1<br>Accept: */*<br></body>
   </html>
   ```

   1. We can determine what public IP address you are using to get to the internet resource by the following command:

   ```
   dig +short myip.opendns.com @resolver1.opendns.com
   ```

   That will return something like:

   ```
   h-4.2$ dig +short myip.opendns.com @resolver1.opendns.com
   54.85.184.24
   ```

   You can also curl the internet facing NLB using the outputs from the cloudformation to get the DNS name.


   ```
   curl gwlblab-NLB-internet-62cb28c8531gf6ba.elb.us-west-2.amazonaws.com
    
   ```

Notice the servers return the http headers that are received by the server. When we route through the Palo Alto firewalls, we will have a policy that inserts an addition header 'PaloAlto'. This is just added for us to test.

We also want to test from our laptop that we can get to our internet facing application.

   1. From the **Outputs** tab of our Stack, in the**Cloudformation** console**, we can get the **AppURL**
   ![Stack Outputs](/images/gwlb-cfn-outputs.png)

   1. You should see the app. Consider using Developer tools in chrome (Option-Control-I on a mac, Control-Shift-I on PC) to look at Network performance, etc. 
   ![Stack Outputs](/images/gwlb-browser-nofw-devtools.png)   

### Troubleshooting

If you have any issues, verify that the Cloudformation stack has complete. It can take a few minutes to finish the automation and the EC2 instance to finish booting up and running through the bootstrapping process.

## You have completed the Lab.

