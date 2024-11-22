Bastion Server is created in public subnet to SSH web EC2 if needed

Below are the steps to SSH web EC2 from our Bastion EC2.

To use Pageant (an SSH authentication agent that comes with PuTTY) to connect from a bastion EC2 instance to a web server EC2 instance, follow these steps:

**Prerequisites**

1. **PuTTY and Pageant Installed**: Ensure you have PuTTY and Pageant installed on your local machine.
2. **SSH Key Pair**: You should have your SSH key pair ready. The private key should be in .ppk format for use with PuTTY/Pageant.

**Steps**

1. **Load Your Private Key into Pageant**:
   - Open Pageant from your taskbar.
   - Click on “Add Key” and select your .ppk private key file. This will load your private key into Pageant.
2. **Configure PuTTY for the Bastion Host**:
   - Open PuTTY.
   - In the “Host Name” field, enter the public IP address of your bastion host.

     <br>
3. **Configure PuTTY for the Web Server**:
   - In PuTTY, Under “Connection” > “SSH” > “Proxy”, enter the private IP address of your web server.
   - Under “Connection” > “SSH” > “Auth”, ensure “Allow agent forwarding” is checked.
   - Under “Connection” > “SSH” > “Proxy”, set the “ProxyCommand” to use the bastion host: plink -agent ec2-user@ bastionserverip-W %h:%p

     <br>
4. **Connect to the Web Server via Bastion Host**:
5. Open PuTTY and load the session for your web server.
   - Click “Open” to start the connection. PuTTY will use Pageant to forward your SSH key through the bastion host to the web server.
   - Enter ec2-user name for login in putty CLI window
   - Enter ssh ec2user@webserverip command to ssh webserver.
