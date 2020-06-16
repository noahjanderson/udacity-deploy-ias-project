# udacity-deploy-ias-project
A project to explore the usage of AWS Cloud Formation Script to be able to quickly create Infrastructure as Code.

# Diagram

![Network and Servers Diagram](https://github.com/noahjanderson/udacity-deploy-ias-project/blob/master/AWS%20VPC%20HA%20APP.png?raw=true)



## Server specs

Uses a **Launch Configuration** for the application servers in order to deploy four servers, two located in each of the private subnets. The launch configuration is used by an auto-scaling group.

Each server has 2 vCPUs and at least 4GB of RAM. The Operating System to be used is Ubuntu 18 and has at least 10GB of disk space allocated.

## Security Groups and Roles

Since the user data script in the launch configuration downloads the application archive from an **S3 Bucket**, we need to create an **IAM Role** that allows our instances to use the S3 Service.
The app  communicates on the default `HTTP Port: 80`, so servers need this inbound port open since we will use it with the Load Balancer and the Load Balancer Health Check. As for outbound, the servers will need unrestricted internet access to be able to download and update its software.

The load balancer should allow all public traffic `(0.0.0.0/0)` on port `80` inbound, which is the default `HTTP port`. Outbound, it will only be using `port 80` to reach the internal servers.

The application needs to be deployed into private subnets with a **Load Balancer** located in a public subnet.

One of the output exports of the **CloudFormation** script should be the public URL of the **LoadBalancer**.

Add the Output of the load balancer **DNS Name** for convenience.
