<h2>Part 1</h2>

![Screen Shot 2023-12-17 at 20 45 52](https://github.com/NikDzub/aws_vpc/assets/87159434/9c60f54c-0889-4743-a929-af9f5d656a0f)

### Create VPC ###

VPC - Virtual Private Cloud is a commercial cloud computing service that provides a virtual private cloud,
by provisioning a logically isolated section of Amazon Web Services Cloud.

<img width="730" alt="Screen Shot 2023-12-17 at 19 44 11" src="https://github.com/NikDzub/aws_vpc/assets/87159434/30fbc2ec-c44c-45d3-a60c-a18f577ad7bf">

### Enable DNS hostnames ###
The DNS hostnames attribute determines whether instances launched in the VPC receive public DNS hostnames that correspond to their public IP addresses.

### Enable Internet gateways & attach to vpc ###
An internet gateway is a virtual router that connects a VPC to the internet.

`aws ec2 attach-internet-gateway --vpc-id "vpc-08c6e4ccb8b131621" --internet-gateway-id "igw-08a034af09f3ce082" --region us-east-1`
### Create Public Subnets ###
To add a new subnet to your VPC, you must specify an IPv4 CIDR block for the subnet from the range of your VPC. You can specify the Availability Zone in which you want the subnet to reside. You can have multiple subnets in the same Availability Zone.
You can optionally specify an IPv6 CIDR block for your subnet if an IPv6 CIDR block is associated with your VPC.

<img width="730" alt="Screen Shot 2023-12-17 at 20 08 00" src="https://github.com/NikDzub/aws_vpc/assets/87159434/cdd15c37-bb4b-4320-8848-56d282de2152">
<img width="1079" alt="Screen Shot 2023-12-17 at 20 12 13" src="https://github.com/NikDzub/aws_vpc/assets/87159434/12355300-8636-4ef0-95f9-0f485b24d9e4">

Edit the subnet settings to enable auto-assign public IPv4 address.

The auto-assign IP settings determines if, when you launch an EC2 instance, the primary network interface is assigned a public IPv4 address or IPv6 address by default.

### Create Route Table & Add routes ###
Once we created the vpc a main route table was created automatically for it, a private one
so lets create a public one as well and add routes

<img width="1234" alt="Screen Shot 2023-12-17 at 20 23 53" src="https://github.com/NikDzub/aws_vpc/assets/87159434/3811f77d-01ac-490d-b22e-ace4f5dd796a">

### Associate the subnets with the route table ###
<img width="1233" alt="Screen Shot 2023-12-17 at 20 26 51" src="https://github.com/NikDzub/aws_vpc/assets/87159434/ca2c326d-5ffe-40b9-a6a7-f6ac60729e2d">
we should now see 2 subnets added to the Explicit subnet associations on the public route table

### Add 4 more private subnets (app + data) ###
keeping in mind to associate the private subnet to the same availability zone as the public one

<img width="1070" alt="Screen Shot 2023-12-17 at 20 51 16" src="https://github.com/NikDzub/aws_vpc/assets/87159434/70e90c11-2566-4914-ac72-3a746a6aa2cf">

what makes the 2 public subnets actually public is that they are associated in the
public route table wich is 0.0.0.0/24

how the 4 remaining are private? bacause when we created the vpc the main route table was created
wich is private, now when a subnet has not been associated with any route tables they auto associate with the main one.

---

<h2>Part 2</h2>

![Screen Shot 2023-12-17 at 22 01 56](https://github.com/NikDzub/aws_vpc/assets/87159434/d35e1141-1b13-4e06-9f94-fed5d149c1fb)

### Create NAT gateway & Private route tables ###
The NAT gateway allows the instances in the private subnets (app + data) to access the internet

The private route table is associated with the private subnets and routes the traffic to the internet trough the NAT gateway

<img width="725" alt="Screen Shot 2023-12-17 at 22 06 17" src="https://github.com/NikDzub/aws_vpc/assets/87159434/0f048e88-59e1-408b-836c-9904425840af">

after creating the nat gateway lets create the private route table and add a route to the NAT

<img width="1245" alt="Screen Shot 2023-12-17 at 22 10 57" src="https://github.com/NikDzub/aws_vpc/assets/87159434/49ac9da2-7776-47de-9b50-5b40c0f55a13">

now lets associate this private route table with the private subnets of az1

<img width="1245" alt="Screen Shot 2023-12-17 at 22 15 18" src="https://github.com/NikDzub/aws_vpc/assets/87159434/97dc5cca-0c79-489d-960e-7a415e467999">

and basically the same for az2 as well

---

<h2>Part 3</h2>

![Screen Shot 2023-12-17 at 22 29 00](https://github.com/NikDzub/aws_vpc/assets/87159434/56bba840-b9e3-49fe-ad2b-a92bad7eab27)

### Creating security groups ###

A security group acts as a virtual firewall for your instance to control inbound and outbound traffic.

lets start with the app load balancer security group

<img width="1223" alt="Screen Shot 2023-12-17 at 22 37 09" src="https://github.com/NikDzub/aws_vpc/assets/87159434/ad363ead-52b6-446b-83cb-24cea6d38745">

then SSH 

<img width="1219" alt="Screen Shot 2023-12-17 at 22 39 03" src="https://github.com/NikDzub/aws_vpc/assets/87159434/42d9d17d-9340-46a0-bbb3-d07587746af1">

then the webserver (adding ssh & alb)

<img width="1220" alt="Screen Shot 2023-12-17 at 22 43 56" src="https://github.com/NikDzub/aws_vpc/assets/87159434/0f448603-68a4-459f-ad2c-8adb5640f45e">

and for the database (port 3306 for mysql)

<img width="1219" alt="Screen Shot 2023-12-17 at 22 47 10" src="https://github.com/NikDzub/aws_vpc/assets/87159434/c027175e-cc4b-4c2a-9094-adf9abe86732">

