+++
title = "Monitoring Traffic"
chapter = false
weight = 50
+++

### Monitoring
We can take a look at the traffic that reaches the firewall and understand what action (allow/deny), what rule caused the action
- Find out your IP address, by browsing to the VPC1 Web Application. Remember its behind a Load balancer. It will indicate your IP address as the **Remote ip**. Record that, 
![VPC 1 Website](/images/test-PA-header.png)
- Switch to the **Monitor** tab at the top
- In the Queury dialog bar, enter the following: `(addr in 205.251.233.178)` and press the arrow to the right. ***Note: since the traffic will be distributed across both Palo Alto, you should perform the query on both appliances.***
![VPC 1 Website](/images/PA-monitor.png)


### Modifying the Header from the Palo Alto
Just so it easily veirifed that the traffic is making its way via the firewalls, we are creating a header **PaloAlto**, this could easily be a more complex key, so that applications can be certain that traffic coming to them have made there way through the firewall. As an extra verfication that the routing policy hasnt been mistakenly changed.

#### Step-by-step
- From the Palo ALto admin Browser console, let's look at the **Policies** by selecting the **Policies** tab and looking at the **Security** policy rules.
![Policies](/images/PA-policies.png)
- From the list of policies click on the **Allow All** rule and switch over to the **Actions** tab. Notice the **Profile Setting** has a **URL Filtering** object attached called **gwlb**.
- Cancel out of the **Security Policy Rule** dialog box, and switch to the **Objects** tab at the top of the console. From the left menu select **URL Filtering** from the **Secuirty Profiles** section.
- From the list of **URL Filtering** profiles, in the **NAME** column click on **gwlb**
![Filter](/images/PA-filter.png)
- In the **URL Filtering Profile** dialog box, switch to the **HTTP Header Insertion** tab.

- In the **NAME** column click on **gwlb** to bring up the **HTTP Header Insertion** dialog box. 
![Header Insertion](/images/PA-header-insertion.png)
- In the **VALUE** column, click **YES** change this to the IP address of this Firewall. 
![Header](/images/PA-header.png)
- Edit the **Value** to send back a unique identifier such as the IP address of the firewall and click the **OK** button. 
- At the **HTTP Header Insertion** dialog box click **OK** button again to save changes.
- At the **URL Filtering Profile** dialog box click **OK** button again to save changes.
- In order to push these changes to the running Policies, click the **Commit** button at the top right of the console page. 
- Repeat these steps for the second appliance using a unique identifier for its header insertion. ***Note: be sure to Commit the changes for them to take effect***

#### Now if you browse to the VPC1 website using the URL provided in the Cloudformation Outputs, you will see the IP address of the Firewall that processesed the connection in the Palo Alto header information.


### You have completed the Monitoring overview.
