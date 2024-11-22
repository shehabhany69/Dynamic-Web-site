**Security**:

- We were able to create the full stack from CLI without actual login to the AWS console
- Create application EC2s inside the private subnet
- Create a Bastion host to be able to SSH to the webapp EC2s only from the Bastion security group.
- Webapp EC2s are behind Nat gateway.
- Created IAM role for the EC2 to access the S3, Load balancer and CloudWatch.
- Created Security group for the EC2, Bastion Host and load balancer.&#x20;

**High Availability:**

- Create machines in 2 different availability zones \[the initial plan is to create 2 EC2s in each AZ, but for demo purposes, we just made 1 EC2 in each AZ.]
- Use the application load balancer to load the balance between the web app EC2s and check the health of instances using the target group.
- Use an auto-scaling group so that if there is an unhealthy instance, ASG will create a replacement for it \[This also can be considered as part of scalability solution!].

**Scalability:**

- Use an auto-scaling group.
- Policies to scale up/down in case the CPU threshold is exceeded and fire an alarm.

**Disaster-Recovery:**

For disaster recovery we will implement our network in another region using Route53 failover routing:

- Open Amazon management console & choose Route53 then Registered Domains, click on Registered Domains then Click **“Check”** to see if the domain is available. If it is, you can proceed with the registration.

1. **Fill in Contact Details**: Provide the necessary contact information for the domain registration. This usually includes your name, address, email, and phone number.
2. **Enable Privacy Protection** (optional): You can choose to enable privacy protection to hide your contact information from the public WHOIS database.
3. **Review and Confirm**: Review your details and the terms and conditions. If everything looks good, click on **“Complete Order”**.

<br>

<br>

<br>

<br>
