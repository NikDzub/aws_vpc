### Create VPC ###
VPC - Virtual Private Cloud is a commercial cloud computing service that provides a virtual private cloud,
by provisioning a logically isolated section of Amazon Web Services Cloud.

<img width="730" alt="Screen Shot 2023-12-17 at 19 44 11" src="https://github.com/NikDzub/aws_vpc/assets/87159434/30fbc2ec-c44c-45d3-a60c-a18f577ad7bf">

### Enable DNS hostnames ###
The DNS hostnames attribute determines whether instances launched in the VPC receive public DNS hostnames that correspond to their public IP addresses.

### Enable Internet gateways & attach to vpc ###
An internet gateway is a virtual router that connects a VPC to the internet.

`aws ec2 attach-internet-gateway --vpc-id "vpc-08c6e4ccb8b131621" --internet-gateway-id "igw-08a034af09f3ce082" --region us-east-1`
