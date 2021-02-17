+++
title = "Test Connectivity"
chapter = false
weight = 79
+++

## Test connectivity through the Firewalls

Now if we have complete all the steps we should be able to test connectivity across the Palo Altos. You will test using your browser as you did when we first deployed the application using **Cloudformation**

![Flow Diagram](/images/vpc1-fwflow-diagram.png)

### Step-by-step

1. From the **Cloudformation** console, select the Stack created for the lab and switch to the **Outputs tab** and on a new tab browse to the URL provided in **AppURL**.

   ![connect button](/images/test-cfn-outputs.png)

1. From your browser connect to the VPC Network Load Balanced application and verify that the header is modified by the firewall to include a new **PaloAlto** header.

   ![Modifed Header](/images/test-PA-header.png)



## You have completed the Lab.
