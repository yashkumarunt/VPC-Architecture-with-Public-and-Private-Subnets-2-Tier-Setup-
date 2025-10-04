# ☁️ VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-

Custom AWS VPC project showcasing a **2-tier architecture** with 🟢 **Public** and 🔵 **Private Subnets**, **Internet Gateway**, **NAT Gateway**, **Bastion Host**, and secure **Database access**.  
Includes 🛣 **Route Tables**, 🔐 **Security Groups**, 🌐 **NACLs**, and 🚧 **Troubleshooting Notes** with full 📸 documentation and 📝 diagrams.

---

### 🎯 **Objective**
- Build a **custom VPC (Virtual Private Cloud)** with public and private subnets, secure routing, NAT Gateway, Bastion Host, and a Database instance.  
- Simulate a **real-world 2-tier architecture**:
  - 🟢 **Public tier** → Bastion Host + Web Servers  
  - 🔵 **Private tier** → Database Servers (no direct internet access)  
- **Region:** `us-east-1 (N. Virginia)`  
- **Availability Zones:** `us-east-1a` & `us-east-1b`

---

### 🧭 **Architecture Diagram**

![VPC Infrastructure](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/VPC%20INFRASTRUCTURE.png?raw=true)

---

### 🏗 **VPC Creation**
- Created a **VPC** named `MyCorpVPC` with a **CIDR block** `192.168.0.0/16`.
- Disabled the default VPC to practice a fully custom setup.

![VPC Overview 1](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/1.vpc%20overview%201.png)

![VPC Overview 2](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/1.%20vpc%20overview%202.png?raw=true)

---

### 🌐 **Subnets Setup**
- Created **4 Subnets** (2 Public, 2 Private) across different AZs:  
  - 🟢 Public Subnet-1 → `192.168.1.0/24` (AZ: us-east-1a)  
  - 🟢 Public Subnet-2 → `192.168.2.0/24` (AZ: us-east-1b)  
  - 🔵 Private Subnet-1 → `192.168.3.0/24` (AZ: us-east-1a)  
  - 🔵 Private Subnet-2 → `192.168.4.0/24` (AZ: us-east-1b)

![Subnets List](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/2.%20Subnets%20List.png?raw=true)

---

### 🛣 **Route Tables**
- **Public Route Table** → Routes internet traffic via IGW 🌐  
- **Private Route Table** → Routes outbound internet via NAT Gateway 🧭  
- 📝 *Issue:* Initially saw “blackhole” after deleting NAT. Learned private subnets lose internet once NAT is removed.

**Public Route Table**  
![Public Route Table](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/3.%20public%20RT.png?raw=true)

**Private Route Table**  
![Private Route Table](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/3.%20private%20RT.png?raw=true)

---

### 🌍 **Internet Gateway (IGW)**
- Created and attached an **Internet Gateway** to the VPC.  
- Ensured public subnets had correct routes.

![Internet Gateway](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/4.%20IG.png?raw=true)

---

### 🧭 **NAT Gateway**
- Created a **NAT Gateway** in Public Subnet-1 with an **Elastic IP**.  
- Connected private subnet via NAT route.  
- ✅ Deleted NAT & Elastic IP later to **avoid extra costs**.

![Elastic IP Allocation Page](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/2.%20Elastic%20IP%20Allocation%20Page.png?raw=true)

![NAT Gateway Details Page](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/1.%20NAT%20Gateway%20Details%20Page.png?raw=true)

---

### 🔐 **Security Groups**
- 🟢 **Bastion Host SG** → Allowed SSH (22) only from my IP.  
- 🔵 **Database SG** → Allowed MySQL (3306) only from Bastion SG.  
- Learned: Security Groups = Stateful ✅ | NACL = Stateless 📝

![Security Group – Bastion](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/6.%20security%20grp%20bastion.png?raw=true)

![Security Group – Database](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/6.%20security%20grp%20databse.png?raw=true)

---

### 🌐 **Network ACL (NACL)**
- Configured **custom inbound/outbound rules**.  
- 🔸 Initially allowed “All traffic” → later refined for best practices.  
- Key learning: **NACL = subnet firewall**, **SG = instance firewall**.

![Network ACL (NACL)](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/8.%20NACL.png?raw=true)

![NACL Inbound Rules](https://github.githubusercontent.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/8.%20NACL%20inbound%20rules.png?raw=true)

![NACL Outbound Rules](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/8.%20NACL%20outbound%20rules.png?raw=true)

---

### 🧑‍💻 **Bastion Host Setup**
- Launched EC2 in **Public Subnet-1** as Bastion Host.  
- Copied `.pem` key from local → Bastion Host.  
- Used Bastion Host to SSH into private EC2 (Database).  
- ✅ Database stays isolated from the internet.

![Bastion Hosting](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/bastion%20hosting%20.png?raw=true)

![PEM File in Bastion Host](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/pem%20file%20in%20bastion%20host.png?raw=true)

![Copying PEM File from PC to Bastion Host](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/copying%20pem%20file%20from%20our%20pc%20to%20bastion%20host.png?raw=true)

---

### 🖥 **EC2 Instances**
- 🟢 **Public Instances** → Web + Bastion, accessible via public IP.  
- 🔵 **Private Instances** → Database, accessible only via Bastion.

![Private EC2 Instance Console](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/4.%20Private%20EC2%20Instance%20Console.png?raw=true)

![Private Instance Terminal - 1](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/5.%20Private%20Instance%20Terminal%20-1.png?raw=true)

![Private Instance Terminal - 2](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/5.%20Private%20Instance%20Terminal%20-2%20.png?raw=true)

![Connection EC2 → Private EC2 (Database)](https://github.com/yashkumarunt/VPC-Architecture-with-Public-and-Private-Subnets-2-Tier-Setup-/blob/main/Screenshots/Connection%20EC2%20to%20Private%20EC2%20database.png?raw=true)

---

### 🧠 **Issues Faced & Troubleshooting**
- ❌ NAT showed “Blackhole” after deletion → 📝 Learned private subnets rely fully on NAT for outbound.  
- 🔐 Couldn’t SSH into Private EC2 → ✅ Used Bastion Host (jump box).  
- 🧭 Route Tables were mixed initially → 📌 Renamed and labeled correctly (Public vs Private).

---

### 📚 **Key Learnings**
- Difference between **IGW vs NAT**.  
- Security Groups 🆚 NACLs.  
- Multi-AZ architecture design.  
- 💰 Cost optimization by deleting NAT & Elastic IP after testing.

---

### 🏁 **Conclusion**
This project provided **hands-on experience** in building a **production-style AWS VPC** with:
- 🧭 Custom networking  
- 🔐 Layered security (SG + NACL)  
- 🧑‍💻 Bastion Host for secure private access  
- 📝 Proper documentation, troubleshooting, and architecture design

🚀 **This setup mirrors real-world Cloud & DevOps scenarios** and is a solid portfolio project.
