+++
title = "Test VPC to VPC Connectivity"
chapter = false
weight = 79
+++

## Test connectivity through the Firewalls

Now if we have complete all the steps we should be able to test connectivity across the Palo Altos. You will test using an EC2 instance in VPC1 and use the curl command to connect to the EC2 instance in VPC2.

![Flow Diagram](/images/internal-test-curl.png)

### Step-by-step

1. From the **Amazon EC2** console and from the left menu select **Instances**.

   ![connect button](/images/gwlb-instances.png)

1. Check the box next to the new instance in the list (named VPC1-Server) and click the **Connect** button above the list.

   ![connect session manager](/images/gwlb-ssm-connect.png)

1. Click the tab **Session Manager** and click the **Connect** button. _This will bring up a new browser tab with the Linux bash shell_

   ![curl VPC2 Server](/images/gwlb-curlvpc1server.png)

1. At the **sh-4.2\$** prompt, curl **10.2.2.10**.

   ```
   curl 10.2.2.10
   ```

   This will return something like the following

   ```
   <html>
   <body>
   Gateway Load Balancer Demo - VPC2 Server.
   Availability Zone: us-west-2a
   Stack Name: gwlblab
   your ip is 10.1.2.10
   <h2>Headers</h2>
   Host: 10.2.2.10<br>User-Agent: curl/7.61.1<br>Accept: */*<br>PaloAlto: YES<br>
   </body>
   </html>
   ```

   1. Notice the **PaloAlto:YES** header near the end of the returned page. We are now sending traffic between the two VPCs through the Palo Alto FWs. 



## You have completed the Lab.
