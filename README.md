# VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-
Custom AWS VPC project showcasing a 2-tier architecture with public and private subnets, Internet Gateway, NAT Gateway, Bastion Host, and secure database access. Includes route tables, security groups, NACLs, and troubleshooting notes with full documentation and diagrams.

**Objective**: Create a custom VPC (Virtual Private Cloud) with public and private subnets, secure routing, NAT Gateway, Bastion Host, and Database instance.
This simulates a real-world 2-tier architecture:
Public tier → Bastion Host + Web Servers
Private tier → Database Servers (no direct internet access)
Region: us-east-1 (N. Virginia), Availability Zones us-east-1a and us-east-1b.

**Architecture Diagram**

![VPC Infrastructure](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/VPC%20INFRASTRUCTURE.png?raw=true)

**VPC Creation**
Created a VPC named MyCorpVPC with a CIDR block 192.168.0.0/16.
Disabled default VPC to practice custom setup.

![VPC Infrastructure](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/VPC%20INFRASTRUCTURE.png?raw=true)

![VPC Overview 2](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/1.%20vpc%20overview%202.png?raw=true)

**Subnets Setup**
4 Subnets created (2 Public, 2 Private across AZs):
Public Subnet-1 → 192.168.1.0/24 (AZ: us-east-1a)
Public Subnet-2 → 192.168.2.0/24 (AZ: us-east-1b)
Private Subnet-1 → 192.168.3.0/24 (AZ: us-east-1a)
Private Subnet-2 → 192.168.4.0/24 (AZ: us-east-1b)

![Subnets List](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/2.%20Subnets%20List.png?raw=true)

**Route Tables**
Public Route Table → Routes internet traffic via Internet Gateway.
Private Route Table → Routes outbound internet via NAT Gateway.
Issues faced: Initially confused because the private route table still showed “blackhole” after deleting NAT. Learned that once NAT is deleted, traffic breaks until replaced.

**Public Route Table**
![Public Route Table](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/3.%20public%20RT.png?raw=true)

**Private Route Table**
![Private Route Table](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/3.%20private%20RT.png?raw=true)

**Internet Gateway (IGW)**
Created and attached an Internet Gateway to the VPC.
Ensured Public Subnets had a route to IGW.

![Internet Gateway](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/4.%20IG.png?raw=true)

**NAT Gateway**
Created a NAT Gateway in Public Subnet-1 with an Elastic IP.
Connected Private Subnet-1 via NAT route.
Later deleted NAT Gateway and Elastic IP to avoid ongoing costs.

![Elastic IP Allocation Page](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/2.%20Elastic%20IP%20Allocation%20Page.png?raw=true)

![NAT Gateway Details Page](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/1.%20NAT%20Gateway%20Details%20Page.png?raw=true)

**Security Groups**
Bastion Host SG → Allowed SSH (port 22) from my PC’s IP.
Database SG → Allowed MySQL (3306) only from Bastion Host SG.
Learned the difference between Security Groups (stateful) vs NACL (stateless).

![Security Group – Bastion](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/6.%20security%20grp%20bastion.png?raw=true)

![Security Group – Database](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/6.%20security%20grp%20databse.png?raw=true)

**Network ACL (NACL)**
Configured custom Inbound and Outbound rules.
Issue faced: Initially allowed “All traffic” but later refined to follow best practices.
Key learning: NACLs act as a firewall at the subnet level, while Security Groups protect at the instance level.

![Network ACL (NACL)](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/8.%20NACL.png?raw=true)

![NACL Inbound Rules](https://github.githubusercontent.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/8.%20NACL%20inbound%20rules.png?raw=true)

![NACL Outbound Rules](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/8.%20NACL%20outbound%20rules.png?raw=true)

**Bastion Host Setup**
Launched an EC2 instance in Public Subnet-1 → Used as Bastion Host.
Copied .pem key file from local system to Bastion Host.
Connected from Bastion Host → Private EC2 (database) instance.
This ensured database is not exposed to public internet.

![Bastion Hosting](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/bastion%20hosting%20.png?raw=true)

![PEM File in Bastion Host](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/pem%20file%20in%20bastion%20host.png?raw=true)

![Copying PEM File from PC to Bastion Host](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/copying%20pem%20file%20from%20our%20pc%20to%20bastion%20host.png?raw=true)


**EC2 Instances**
Public Instances (Web / Bastion) → Accessible via public IP.
Private Instance (Database) → No public IP, accessed only through Bastion Host.

![Private EC2 Instance Console](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/4.%20Private%20EC2%20Instance%20Console.png?raw=true)

![Private Instance Terminal - 1](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/5.%20Private%20Instance%20Terminal%20-1.png?raw=true)

![Private Instance Terminal - 2](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/5.%20Private%20Instance%20Terminal%20-2%20.png?raw=true)

![Connection EC2 → Private EC2 (Database)](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/Connection%20EC2%20to%20Private%20EC2%20database.png?raw=true)


**Issues Faced & Troubleshooting**
Problem: NAT showed “Blackhole” after deletion.
Learning: Private subnets lose outbound access once NAT is deleted.

Problem: Couldn’t SSH directly into Private EC2.
Solution: Used Bastion Host for jump connection.

Problem: Initially mixed up Route Tables.
Solution: Labeled them clearly in diagram → Public vs Private.

**Key Learnings**
Difference between IGW vs NAT.
Role of Security Groups vs NACLs.
How to design highly available architecture across multiple AZs.
Importance of cost optimization (deleted NAT + Elastic IP to save money).

**Conclusion**
This project gave me hands-on experience in building a production-like AWS VPC setup with public/private subnets, secure access via Bastion Host, NAT, IGW, Security Groups, and NACLs.
