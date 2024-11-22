Main participant: [Manar Mostafa](https://app.nuclino.com/users/6e8a3d15-0503-4f48-ab61-17d24844d642?mId=9rQ2KXoK)

Here are the steps to create an Application Load Balancer and Auto-scaling Group to launch EC2 instances if one is unhealthy or the CPU exceeds a specific threshold.

# Create an Application Load Balancer

In EC2 console

1. In that left navigation pane, choose **Load Balancers**. (You might need to scroll down to find it.)
2. Choose **Create load balancer**.
3. For Application Load Balancer, choose Create
4. In the **Basic configuration** section, for Load balancer name, enter `R53RLoadBalancer`
5. In the **network mapping** section, configure the following options:
   - For VPC, Select `route53rangersVpc`
   - For Mappings, choose the first Availability Zone, and then choose the Public Subnet that displays.
   - For Mappings, also choose the second Availability Zone, and then choose the Public Subnet that displays.
6. In the **security group** section, select only `ALBSG`
7. In the **listeners and routing** section, choose the Create target group link.

   <br>

   Now a new browser tab opens

   <br>
8. &#x20;For **Step 1: Specify group details**, configure the following options
   - In the **Basic configuration** section, configure the following options:
     1. **Choose a target type:** Choose **Instances**.
     2. **Target group name:** Enter `project-tg`
     3. &#x20;**VPC:** Ensure that **route53rangerVpc** is chosen.
   - In the **Health checks** section, expand **Advanced health check settings**, and configure the following options:
     1. **Healthy threshold:** Enter `2`.
     2. **Interval:** Enter `10` (seconds).
9. Choose **Next**.
10. The **Step 2: Register targets** screen appears, there is no web application instances yet, so you can skip this step.
11. Review the settings, and choose **Create target group**.

    <br>

    A *Successfully created the target group* message displays.

    <br>
12. Return to the browser tab where you already started defining the load balancer.
13. In the **Listeners and routing** section, choose the refresh icon.
14. From the **Default action** dropdown list, choose the **project-tg** target group that you just created.
15. Scroll to the bottom of the page, and choose **Create load balancer**.

The load balancer is successfully created.

# Create a Launch Template

In EC2 console

1. In that left navigation pane, choose **Launch Template**.
2. Choose **Create Launch template**.
3. To create the launch template, configure the following options:
   - In the **Launch template name and description** section, configure the following options:
     1. For **Launch template name**, enter `R53LaunchTemplate`
     2. For **Auto Scaling guidance**, select **Provide guidance to help me set up a template that I can use with EC2 Auto Scaling.**
   - In the **Application and OS Images (Amazon Machine Image)** section, configure the following options:
     1. Choose ***Quick Start*** **AMIs**
     2. Select **Amazon Linux 2023 AMI**
   - In the **Instance type** section, choose **t2.micro**.
   - In the **Key pair (login)** section, for **Key pair name**, choose **AWS\_KEY2**.
   - In the **Network settings** section, configure the following options:
     1. For **Firewall (security groups),** choose **Select existing security group**.
     2. For **Security groups**, choose **WebAppSG**.
   - Expand the **Advanced details** section, and configure the following options:
     1. For **Detailed CloudWatch monitoring**, choose **Enable**. **Note:** This option allows Auto Scaling to react quickly to changing utilization.
     2. In the **User data** box, enter the script from the YAML file.
4. Choose **Create launch template**.

Next, you create an Auto Scaling group that uses this launch template. The Auto Scaling group defines where to launch the EC2 instances.

# Create Auto-scaling Group

1. In that left navigation pane in the EC2 console, choose **Auto Scaling group**. (You might need to scroll down to find it.)
2. Choose **Create Auto Scaling group.**
3. For **Step 1: Choose launch template or configuration**, configure the following options:
   - **Auto Scaling group name:** Enter `project-ASG`
   - **Launch template**: Confirm that the **R53LaunchTemplate** template that you just created is selected.
   - In the **Version,** choose **Latest (1).**
4. Choose **Next**.
5. For **Step 2: Choose instance launch options**, configure the following options:
   - **VPC**: Choose **route53rangersVpc**.
   - **Availability Zones and subnets**: Choose **both private subnets.** (These settings launch EC2 instances in private subnets across both Availability Zones.)
6. Choose **Next**.
7. For **Step 3: Configure advanced options**, configure the following options:
   - In the **Load balancing** section, configure the following options:
     1. Choose **Attach to an existing load balancer**.
     2. From the **Existing load balancer target groups** dropdown list, choose **project-tg**.
   - In the **Health checks** section, configure the following options:
     1. Select **Turn on Elastic Load Balancing health checks**.
     2. For **Health check grace period**, leave it `300` seconds.
   - In the **Additional settings** section, select **Enable group metrics collection within CloudWatch**. (This setting captures metrics at 1-minute intervals, which allows Auto Scaling to react quickly to changing usage patterns.)
8. Choose **Next**.
9. For **Step 4: Configure group size and scaling policies**, configure the following options:

   - For **Group size**, for **Desired capacity**, enter `2`.
   - For **Scaling**, configure the following options:
     1. For **Min desired capacity**, enter `2`.
     2. For **Max desired capacity,** enter `3`.

   <br>

   These settings allow Auto Scaling to automatically add or remove instances, always keeping 2–3 instances running.

   <br>

   - For **Automatic scaling - *optional***, choose **No scaling policies.**
10. Choose **Next**.
11. For **Step 5: Add notifications**, choose **Add notification**, and configure the following options:
    - For **SNS Topic**, choose `NotificationTopic`
    - For **Event Type**, make sure that the four boxes are selected.
12. For **Step 6: Add tags**, choose **Add tag**, and configure the following options:
    - **Key:** Enter `Name`
    - **Value:** Enter `webapp`
13. choose **Add tag**, and configure the following options:
    - **Key:** Enter `Project`
    - **Value:** Enter `DEPI`
14. Choose **Next**.
15. For **Step 7: Review**, review the details of your Auto Scaling group, and then choose **Create Auto Scaling group**.
16. Choose the auto scaling group you just created and go to the **Automatic scaling** tab
17. For **Dynamic Scaling polices** choose **create Dynamic Scaling polices**
    - For **Policy type**, choose **Simple scaling**
    - For **Scaling policy name**, enter `WebServerScaleDownPolicy`
    - For **CloudWatch alarm**, click on **Create a CloudWatch alarm** this will open a new tab.
      1. In the **Metric** section
         - Choose **Select Metrics**.
         - Search for with the word “CPUUtilization” and choose **by Auto scaling group**
         - Choose the name of the autoscaling group that you created
         - Click **Select Metrics**.
      2. In the **Conditions** section
         - **Threshold type:** static
         - **Whenever StatusCheckFailed\_Instance is...:** Lower
         - **than…:** 20
      3. Choose **Next**.
      4. For **Auto scaling action**, choose **Add  Auto scaling action**
         - For **select a group**, choose the name of the auto scaling group
         - For **Take the following action...,** choose `WebServerScaledownPolicy`
      5. Choose **Next**.
      6. Alarm Name: `CPUAlarmLow`
      7. Choose **Next**.
      8. Scroll to the bottom of the page, and choose **Create Alarm**.
    - Now return to the autoscaling group tab and refresh the alarm section and choose the **CPUAlarmLow**
    - For **Take the action**, enter **remove 1 capacity units**
18. Choose **create.**

    <br>

    Now we do another one for the scale up police

    <br>
19. For **Dynamic Scaling polices** choose **create Dynamic Scaling polices**
    - For **Policy type**, choose **Simple scaling**
    - For **Scaling policy name**, enter `WebServerScaleupPolicy`
    - For **CloudWatch alarm**, click on **Create a CloudWatch alarm** this will open a new tab.
      1. In the **Metric** section
         - Choose **Select Metrics**.
         - Search for with the word “CPUUtilization” and choose **by Auto scaling group**
         - Choose the name of the autoscaling group that you created
         - Click **Select Metrics**.
      2. In the **Conditions** section
         - **Threshold type:** static
         - **Whenever StatusCheckFailed\_Instance is...:** Greater
         - **than…:** 75
      3. Choose **Next**.
      4. For **Auto scaling action**, choose **Add  Auto scaling action**
         - For **select a group**, choose the name of the auto scaling group
         - For **Take the following action...,** choose `WebServerScaleupPolicy`
      5. Choose **Next**.
      6. Alarm Name: `CPUAlarmHigh`
      7. Choose **Next**.
      8. Scroll to the bottom of the page, and choose **Create Alarm**.
    - Now return to the autoscaling group tab and refresh the alarm section and choose the **CPUAlarmHigh**
    - For **Take the action**, enter **add 1 capacity units**
20. Choose **create.**
