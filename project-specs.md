## Server specs

For this project we created a Launch Configuration for the application servers in order to deploy four servers, two located in each private subnet. The launch configuration is used by an auto-scaling group.

This uses two vCPUs and at least 4GB of RAM. The Operating System is Ubuntu 18 for the Machine Image (AMI).

## Security Groups and Roles

Since the application is downloading from an *S3 Bucket*, we needed to create an *IAM* Role that allows the instances to use the S3 Service.

Udagram communicates on the default `HTTP Port: 80`, so the servers needed this inbound port open since it is used with the `Load Balancer` and the `Load Balancer Health Check.` As for outbound, the servers needed unrestricted internet access to be able to download and update its software.

The *load balancer* allows all public traffic `(0.0.0.0/0)` on `port 80` inbound, which is the default `HTTP port`. Outbound, it used `port 80` to reach the internal servers.

The application needed to be deployed into private subnets with a *Load Balancer* located in a public subnet.

One of the output exports of the *CloudFormation* script is the public URL of the *LoadBalancer.*

As a bonus we've add `http://` in front of the load balancer *DNS Name* in the output, for convenience.

# Other Considerations

You can deploy your servers with an *SSH Key* into Public subnets while you are creating the script. This helps with troubleshooting. Once done, move them to your private subnets and remove the *SSH Key* from your *Launch Configuration.*

It also helps to test directly, without the load balancer. Once you are confident that your server is behaving correctly, increase the instance count and add the load balancer to your script.

While your instances are in public subnets, you'll also need the `SSH port open (port 22)` for your access, in case you need to troubleshoot your instances.

Log information for UserData scripts is located in this file: `cloud-init-output.log` under the folder: `/var/log.`

You should be able to destroy the entire infrastructure and build it back up without any manual steps required, other than running the *CloudFormation* script.

The provided UserData script should help you install all the required dependencies. Bear in mind that this process takes several minutes to complete. Also, the application takes a few seconds to load. This information is crucial for the settings of your load balancer health check.

It's up to you to decide which values should be parameters and which you will hard-code in your script.

See the provided supporting code for help and more clues.

If you want to go the extra mile, set up a bastion host to allow you to SSH into your private subnet servers. This bastion host would be on a Public Subnet with `port 22` open only to your home `IP address`, and it would need to have the private key that you use to access the other servers.

Last thing: Remember to delete your *CloudFormation* stack when you're done to avoid recurring charges!