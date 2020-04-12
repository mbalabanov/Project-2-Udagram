# Project-2-Udagram
Deploying an application, along with the necessary supporting software into its matching infrastructure.

## Infrastructure specs

For this project we created a Launch Configuration for the application servers in order to deploy four servers, two located in each private subnet. The launch configuration is used by an auto-scaling group.

This uses two vCPUs and at least 4GB of RAM. The Operating System is Ubuntu 18 for the Machine Image (AMI).

## Security Groups and Roles

Since the application is downloading from an *S3 Bucket*, we needed to create an *IAM* Role that allows the instances to use the S3 Service.

Udagram communicates on the default `HTTP Port: 80`, so the servers needed this inbound port open since it is used with the `Load Balancer` and the `Load Balancer Health Check.` As for outbound, the servers needed unrestricted internet access to be able to download and update its software.

The *load balancer* allows all public traffic `(0.0.0.0/0)` on `port 80` inbound, which is the default `HTTP port`. Outbound, it used `port 80` to reach the internal servers.

The application needed to be deployed into private subnets with a *Load Balancer* located in a public subnet.

One of the output exports of the *CloudFormation* script is the public URL of the *LoadBalancer.*

As a bonus we've add `http://` in front of the load balancer *DNS Name* in the output, for convenience.
