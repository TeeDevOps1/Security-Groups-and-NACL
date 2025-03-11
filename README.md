# Security-Groups-and-NACLs
Understanding the fundamental components of AWS infrastructure, including  how security groups control inbound and outbound traffic to EC2 instance how NACLs act as subnet level firewalls

## Create a new repository
i created a new repository (Security Groups and NACLs) and cloned it into the local repository
![1](./img/1.png)
![2](./img/2.png)

## create an EC2 instance
i created an EC2 instance (my-public-1) in the public subnet to host the website, the security group configuration for the instance had its inbound rule set at IPv4 SSH traffic on port 22, while the outbound rule was allowed traffic on any aport meaning it had unrestricted access to the internet 
![3](./img/3.png)
![4](./img/4.png)
testing the website with the ip address (54.158.129.248) on the browser i noticed it was not able to connect saying the site can not be reachhed as seen below:
![7](./img/7.png)
![8](./img/8.png)
this was due to the fact that the HTTP protocol in the security group was not defined so whenever the outside word tries to access the instance it will be unsuccessful in order to rectify this we would need to configure the Security Group to allow HTTP traffic following the steps in the diagram below:
the first thing is to create a Security Group with the right parameters and configure the inbound and outbound rules,
add the inbound rule type "SSH" and "HTTP" using 0.0.0.0/0 as the CIDR Block thereby allowing all for the inbound rules  while leaving the outbound rules as default
![9](./img/9.png)
![10](./img/10.png)
![11](./img/11.png)
![14](./img/14.png)
After successfully configuring the security group the next step is to attach it with the EC2 instance
![12](./img/12.png)
![13](./img/13.png)
the next step is to try the website / public ip for the EC2 instance (54.158.129.248) which as we can see below worked successfullly
![29](./img/29.png)
from the result above we can see the configuration of the Security Group allows the "HTTP" and "SSH" access to the instance.
 if we remove the outbound rule then there would be no acccess to the outside for this instance
![16](./img/16.png)
![29](./img/29.png)
although we removed the access of the outbound rule expecting the access to the instance to be invalid however the website was still accessible this is because  Security Groups are stateful meaning they automatically allow return traffic initiated by the instances to which they are attached to ,hence we can still access it even after removing the outbound rule.
if we delete both the inbound rule and outbound rule though we would not be able to access the website as shown below:
![15](./img/15.png)
 In conclusion in other to access our website the Security Group must be configured properly most importantly necessary protocols for thhe inbound rules such as "SSH" and "HTTP" must be added


 ## Network Access Control List (NACL)
  The NACL  controls the traffic entering and exiting the subnet unlike the Security Group it stateless meaning it does not automatically allow returning traffic access to the instance.
   Navigating to the VPC section on AWS management console we locate and click on the Network ACL where we create a network ACLs (my-first-NACL) choosing the VPC "my-vpc-1-vpc":
   ![18](./img/18.png)
   ![19](./img/19.png)

   Navigating to the "Inbound rules" and "outbound rules" section which by default denys all traffic from all ports
   ![20](./img/20.png)
   ![21](./img/21.png)
   i edit the "inbound rules" and add a new rule setting the type to all traffic and allow all traffic for the source (0.0.0.0/0)
   ![20a](./img/20a.png)
   afterwhich i associate it with my public subnet as the instance resides on the public subnet and allow all traffic on the "inbound rule":
   ![23](./img/23.png)
   ![24](./img/24.png)
   ![25](./img/25.png)
   ![26](./img/26.png)
   As soon as the NACL is attached to my public subnet i try accessing the website again but was unable to see it, even though the inbound rule allows traffic the outbound rule are still denying all traffic, This is because NACLs are stateless and do no not automatically aallow return traffic as a result the rules for both the inbound and outbound traffic must be configuredexplicitly:
   ![29](./img/15.png)
   ![27](./img/27.png)
   i was not able to see the website because the inbound rule(allow all) but any traffic from the subnet is not allowed to go outside due to the outbound rule(deny all), but by allowing the outbound rule (i.e change to allow all) to go outside we will be able to access when the website is revisited as shown below:
   ![28](./img/28.png)
   ![29](./img/29.png)
