# Migration of Web Server from AWS to OCI
This project focused on moving a fully operational web server and its domain from AWS to Oracle Cloud Infrastructure (OCI). The migration was done using a secure site-to-site VPN tunnel to transfer data between AWS and OCI with minimal downtime. The web server, which was running on AWS EC2 with Nginx, was set up again on OCI compute instances. The domain configuration was also updated to direct traffic to the new OCI infrastructure.

Image transfer between Amazon S3 and OCI Object Storage was performed using the open-source tool rclone, ensuring all server files and configurations were moved efficiently. After the migration, thorough testing was done to confirm the web server was working correctly and the domain was resolving properly in the new OCI setup.

# Step 1 - Check the existing VPC and it's IPV4 CIDR range, Web Server and record hosted in the hosted zone.
Log in to the Oracle Cloud Infrastructure (OCI) console. Set up a Virtual Cloud Network (VCN), configure subnets, and prepare a compute instance for hosting the web server. Confirm the web server is running and identify its IP address. Verify that the domain (rqcloudmigrationexpert.online) is correctly configured and hosted. Take note of the DNS records and validate the domain's functionality.

![Screenshot from 2024-12-04 13-06-28](https://github.com/user-attachments/assets/9c03cec1-6023-4208-b600-48232fd81a78)
![Screenshot from 2024-12-04 13-41-30](https://github.com/user-attachments/assets/451151cb-6b9c-437f-96fa-0a0c817a18dd)
![Screenshot from 2024-12-07 01-26-38](https://github.com/user-attachments/assets/10d47b19-45de-4fbd-85e6-aadfad85c7d6)

Check if the domain is working fine and if the traffic is directed to the public IP of the Web Server.

![Screenshot from 2024-12-07 01-28-09](https://github.com/user-attachments/assets/01c7c245-dbd0-4450-a015-e3e2a4ee7969)

# Step 2 - Create Customer Gateway (CGW)
CGW represents OCI’s network to AWS, allowing AWS to know where to send VPN traffic. Create a customer gateway (CGW) as per the configuration mentioned in below screenshot. Give '31898' as BGP ASN and dummy ip '1.1.1.1' as of now. Please note that we need to give the public IP of OCI tunnel to CGW which we are going to configure in further steps. Since we need to download the configuration file we are giving the dummy IP.

![Screenshot from 2024-12-07 01-30-46](https://github.com/user-attachments/assets/85dbe969-50b6-42a5-a96f-8e8ed421a676)

# Step 3 - Create Virtual Private Gateway (VGW)
The Virtual Private Gateway (VGW) is the AWS-managed VPN endpoint on the AWS side of the connection. It serves as the AWS-side termination point for VPN connections.
Create Virtual Private Gateway and then attach it to the VPC. 

![Screenshot from 2024-12-07 01-31-22](https://github.com/user-attachments/assets/44155df3-a9e4-46e8-a6b6-42ce99585515)

# Step 4 - Create Site to Site VPN connection and download the configuration
A VPN Connection is the actual secure link established between the AWS VPC and the OCI Virtual Cloud Network (VCN) through the internet.
Create a VPN connection and per the configuration mentioned in below screenshot. Make sure to select the CGW and VGW

![Screenshot from 2024-12-07 14-45-34](https://github.com/user-attachments/assets/5798b464-5a3c-49b5-be9b-0cdfb137f3ad)

Select the option as mentioned in below screenshot and download the configuration file.

![Screenshot from 2024-12-07 15-01-33](https://github.com/user-attachments/assets/e0fc2859-bafa-427d-b715-3e7ce9623413)

## Open the configuration file, make sure to note down the below parameters which we will use in further steps (The values will be different in your case).

![image](https://github.com/user-attachments/assets/2256280f-562f-46f5-843d-4c6729a4b1fc)

## In the above steps we created the VPC, CGW and VGW in AWS. Now we would need to create the same in OCI. The concept and functionality is same but the name changes on OCI side. Please refer below.

Virtual Private Cloud (VPC) --> Vrtual Cloud Network (VCN)
Virtual Private Gateway (VGW) --> Dynamic Rouing Gateway (DRG)
Customer Gateway (CGW) --> Customer Premise Equipment (CPE)





































