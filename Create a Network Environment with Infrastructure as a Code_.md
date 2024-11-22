Main participant: [Marwa ES](https://app.nuclino.com/users/8ef922cc-91e7-4b48-b02d-8bc4522e8fcf?mId=hd8DOpHZ)

In the below procedures, we will create key pair for EC2 access & create VPC in two availability zones, each with two subnets, one is private & the other one is public with public route table & internet gateway & we will add Nat gateway to connect private network to internet .

- Open Amazon management console & get AZ name & another one in the same region (to perform high availability)
- Update Networkstack.yml with these AZ names.
- Create key pair and save ppk on your pc for EC2 SSH connections.
- Open Amazon Management console then EC2 then Key pair
- Create key pair & in Name: ‘**AWS\_KEY2**’ then click create key pair.

  <br>
- &#x20;Open Amazon Management console then open cloudformation then stack then choose create stack
- Choose upload a template file & choose ‘Networkstack.yml’ and upload it then click next
- Name your stack as ‘**R53Rnetwork**’ then click submit.

  <br>

  **The above Network stack formation procedures are from Amazon management console but we will use AWS CLI to run our stacks**

  <br>

<br>
