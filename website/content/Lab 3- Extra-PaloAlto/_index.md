+++
menutitle = "Lab 3: Palo Alto"
chapter = false
weight = 50
+++

# Palo Alto
While this workshop focus on the AWS infrastrucutre and traffic flow, its always good to have some basic understanding of whats going on in the middle. In real-world deployments cusotmers would use a management solution to provide a single pane of glass and control across multiple Palo Alto appliances. In this lab we will configure and view each of the two independently. This mean we will repeat the tasks for each firewall.

### Prerequisite: Set the Password
#### Step-by-step
- Launch **AWS CloudShell**:
  - From the console search for and select  **AWS CloudShell**. After a minute or so a new cloud based shell environment will launch, allowing you to use AWS cli tools and other bash based tools like dig
   ![Cloudshell](/images/gwlb-cloudshell-open.png)
- Upload pem file to cloudshell:
  - From the **Actions** menu in the upper right, select **Upload file**. ![Upload](/images/gwlb-cloudshell-upload-menu.png)
  - Click the **Select file** button.
  - Search for a select the **.pem** file you download during **Lab 1**. Click the **Open** button. ![Install](/images/gwlb-cloudshell-selectFile.png)
  - Move and secure the keym (replace with the name of your key file)
    - run `mkdir ~/.ssh`
    - run `mv key.pem ~/.ssh/`
    - run `chmod 400 ~.ssh/`
    ![Cloudshell](/images/gwlb-cloudshell-key-move.png)
- Install tools
  - run `sudo yum install -y bind-utils`
- SSH into the first Palo Alto
  - the SSH command can be found in the **Cloudformation** console. Select the **stack** and change to the **outputs** tab. ***there will be two ssh commands, one for each appliance***
  - run the copied command, `ssh -i .ssh/***your_key*** admin@***Public_IP_of_the_Palo***
  - ***Note: you may see that someone attempted to login into your appliance if you did not lock down the CIDR for management when deploying the stack. If you want to change this, you can edit the Secuirty group, from the EC2 console, that contains **xVMSeriesManagementSecurityGroup** in the name.***
- Enter configuration mode
  - run  `configure`
- Set admin password
  - run `set mgt-config users admin password`
  - at the **enter password** prompt enter a new password (don t forget it, or you will have to do this step again). Requires 
  - enter the password again at the **Confirm Password** prompt to confirm

### Login to the Palo Alto Console
- the Palo Alto URL can be found in the **Cloudformation** console. Select the **stack** and change to the **outputs** tab. ***there will be two URLs, one for each appliance***
- From your browser go to the copied URL, and login with user **admin** and the password you just set.
![PaloAlto Console](/images/gwlb-palo-login.png)
  - ***Note: since we have not loaded a certificate with a varifyable name, the session will have to proceed without Validation. With Google Chrome this means clicking the **Advanced** button and click the **proceed to ***your_Firewalls_IP_Address*** link.

### Repeat the above steps for the second appliance listed in the **Cloudformation** Stack's **Output**







