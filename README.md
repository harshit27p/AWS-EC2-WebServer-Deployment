# Assignment 1: Launch a Web Server in a Custom VPC

## Objective
Learn how to set up a **Virtual Private Cloud (VPC)**, run an **EC2 instance**, and host a simple **webpage**.

---

## Tasks
• Create a custom VPC with:
o 1 public subnet
o Internet Gateway attached
• Launch an EC2 instance (Amazon Linux or Ubuntu) in the public subnet.
• Configure a Security Group to allow SSH (port 22) and HTTP (port 80).
• Install and host a sample Apache or Nginx web page ("Hello from EC2").
• Access the web page using the EC2's public IP.

---

## Steps

### 1. Create a Custom VPC
1. Go to the **VPC Dashboard** in AWS.
2. Click **Create VPC** → Select *VPC only*.
3. Provide:
   - Name: `MyCustomVPC`
   - IPv4 CIDR block: `10.0.0.0/16`
4. Create.

---

### 2. Create Subnets
1. Inside the VPC, create a **public subnet**:
   - CIDR block: `10.0.1.0/24`
   - Availability Zone: Select any (e.g., `us-east-1a`).
   - Enable **Auto-assign public IPv4**.
2. (Optional) Create a private subnet for future use.

---

### 3. Configure Internet Gateway
1. Create an **Internet Gateway (IGW)**.
2. Attach the IGW to your `MyCustomVPC`.

---

### 4. Configure Route Table
1. Create a new **Route Table**.
2. Associate the **public subnet** with this route table.
3. Add a route:
   - Destination: `0.0.0.0/0`
   - Target: Internet Gateway (IGW created above).

---

### 5. Launch an EC2 Instance
1. Go to **EC2 → Launch Instance**.
2. Choose **Amazon Linux 2 AMI**.
3. Instance type: `t3.micro` (Free tier).
4. Network settings:
   - Select **MyCustomVPC**
   - Subnet: `Public Subnet`
   - Enable Auto-assign Public IP
5. Add a **Security Group**:
   - Allow **SSH (22)** from your IP or (anywhere for testing)
   - Allow **HTTP (80)** from anywhere
6. Launch with a new or existing **key pair**.

---

### 6. Connect to the EC2 Instance
- From CloudShell or local terminal:
```bash
ssh -i mykey.pem ec2-user@<EC2_PUBLIC_IP>
```

---

### 7. Install & Start Apache Web Server
  - Run the following commands:
```bash
# Update packages
sudo yum update -y

# Install Apache (httpd)
sudo yum install -y httpd

# Start the service
sudo systemctl start httpd

# Enable auto start on reboot
sudo systemctl enable httpd
```

---

### 8. Create a Simple Web Page

```bash
echo "<h1>Hello from EC2</h1>" | sudo tee /var/www/html/index.html
```

### 9. Test the Web Server
  - Open a browser and visit:

```cpp
http://<EC2_PUBLIC_IP>
```
