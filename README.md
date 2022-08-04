# Module 4: Networking

[AWS Cloud Practitioner Home](https://github.com/pslucas0212/AWS-Cloud-Practioner)

### Introduction

Amazon Virtual Private Cloud (VPC) - Logically isolated section of the Amazon cloud in a virtual network.

Public subnet vs. private subenet

#### Transcript
If we think back to our coffee shop or AWS account, things by now should be running smoothly. Although, what if we had a few eager customers who wanted to give their orders directly to the baristas instead of the cashier out in front? Well, it doesn't make sense to allow every customer to be able to interact with our baristas since they are focused on brewing some fresh caffeinated beverages. So what do we do? 

That's right, kids, say it with me, Amazon Virtual Private Cloud, or VPCs, as they're affectionately known. A VPC lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. These resources can be public facing so they have access to the internet, or private with no internet access, usually for backend services like databases or application servers. The public and private grouping of resources are known as subnets and they are ranges of IP addresses in your VPC. 

Now, in our coffee shop, we have different employees and one is a cashier. They take customers' orders and thus we want customers to interact with them, so we put them in what we call a public subnet. Hence they can talk to the customers or the internet. But for our baristas, we want them to focus on making coffee and not interact with customers directly, so we put them in a private subnet. 

Okay, let's get into the next video where we'll dive into networking.

### Connectivity to AWS

Virtual Private Cloud (VPC) - virtual private network for your AWS resources where you define you own subnets.  Subnets allow you to group your resources together.  You can control what can access your VPCs.

Public facing resources.   To allow public access to your VPC, you need to use an Internet Gateway (IGW)

VPC With private resources.  We don't use a IGW, we use a "private gateway" called a virtual private gateway.  We create a VPN to access the VPC.  You must authenticdate in to you network.  You use a virtual private gateway.

AWS Direct Connect allows you to establish a directly connected fiber network from your business to AWS.  Work with your local fiber provider to configure/setup.


#### Transcript
A VPC, or Virtual Private Cloud, is essentially your own private network in AWS. A VPC allows you to define your private IP range for your AWS resources, and you place things like EC2 instances and ELBs inside of your VPC. 

Now you don't just go throw your resources into a VPC and move on. You place them into different subnets. Subnets are chunks of IP addresses in your VPC that allow you to group resources together. Subnets, along with networking rules we will cover later, control whether resources are either publicly or privately available. What we haven't told you yet is there are actually ways you can control what traffic gets into your VPC at all. What I mean by this is, for some VPCs, you might have internet-facing resources that the public should be able to reach, like a public website, for example. 

However, in other scenarios, you might have resources that you only want to be reachable if someone is logged into your private network. This might be internal services, like an HR application or a backend database. First, let's talk about public-facing resources. In order to allow traffic from the public internet to flow into and out of your VPC, you must attach what is called an internet gateway, or IGW, to your VPC. 

An internet gateway is like a doorway that is open to the public. Think of our coffee shop. Without a front door, the customers couldn't get in and order their coffee, so we install a front door and the people can then go in and out of that door when coming and going from our shop. The front door in this example is like an internet gateway. Without it, no one can reach the resources placed inside of your VPC. 

Next, let's talk about a VPC with all internal private resources. We don't want just anyone from anywhere to be able to reach these resources. So we don't want an internet gateway attached to our VPC. Instead, we want a private gateway that only allows people in if they are coming from an approved network, not the public internet. This private doorway is called a virtual private gateway, and it allows you to create a VPN connection between a private network, like your on-premises data center or internal corporate network to your VPC. 

To relate this back to the coffee shop, this would be like having a private bus route going from my building to the coffee shop. If I want to go get coffee, I first must badge into the building, thus authenticating my identity, and then I can take the secret bus route to the internal coffee shop that only people from my building can use. So if you want to establish an encrypted VPN connection to your private internal AWS resources, you would need to attach a virtual private gateway to your VPC. 

Now the problem with our super secret bus route is that it still uses the open road. It's susceptible to traffic jams and slowdowns caused by the rest of the world going about their business. The same thing is true for VPN connections. They are private and they are encrypted, but they still use a regular internet connection that has bandwidth that is being shared by many people using the internet. 

So what I've done to make things more reliable and less susceptible to slowdowns is I made a totally separate magic doorway that leads directly from the studio into the coffee shop. No one else driving around on the road can slow me down because this is my direct doorway; no one else can use it. What, did you not have a secret magic doorway into your favorite coffee shop? All right, moving on. The point being is you still want a private connection, but you want it to be dedicated and shared with no one else. You want the lowest amount of latency possible with the highest amount of security possible. 

With AWS, you can achieve that using what is called AWS Direct Connect. Direct Connect allows you to establish a completely private, dedicated fiber connection from your data center to AWS. You work with a Direct Connect partner in your area to establish this connection, because like my magic doorway, AWS Direct Connect provides a physical line that connects your network to your AWS VPC. This can help you meet high regulatory and compliance needs, as well as sidestep any potential bandwidth issues. It's also important to note that one VPC might have multiple types of gateways attached for multiple types of resources all residing in the same VPC, just in different subnets. 

Thanks for listening. I'm gonna sit here and keep ordering coffees from my magic door. See ya.



### Subnets and Network Access Control Lists



#### Transcript
Welcome to your VPC. You can think of it as a hardened fortress where nothing goes in or out without explicit permission. You have a gateway on the VPC that only permits traffic in or out of the VPC. But that only covers perimeter, and that's only one part of network security that you should be focusing on as part of your IT strategy. 

AWS has a wide range of tools that cover every layer of security: network hardening, application security, user identity, authentication and authorization, distributed denial-of-service or DDoS prevention, data integrity, encryption, much more. We're gonna talk about a few of these key pieces. And if you really wanna know more about security, please follow the links in this page to be directed to more information and additional training on how to lock your infrastructure down tighter than Aunt Robin's secret lemon pie recipe, and ain't nobody getting that recipe. 
Today, I wanna talk about a few aspects of network hardening looking at what happens inside the VPC. Now, the only technical reason to use subnets in a VPC is to control access to the gateways. The public subnets have access to the internet gateway; the private subnets do not. But subnets can also control traffic permissions. Packets are messages from the internet, and every packet that crosses the subnet boundaries gets checked against something called a network access control list or network ACL. This check is to see if the packet has permissions to either leave or enter the subnet based on who it was sent from and how it's trying to communicate. 

You can think of network ACLs as passport control officers. If you're on the approved list, you get through. If you're not on the list, or if you're explicitly on the do-not-enter list, then you get blocked. Network ACLs check traffic going into and leaving a subnet, just like passport control. The list gets checked on your way into a country and on the way out. And just because you were let in doesn't necessarily mean they're gonna let you out. Approved traffic can be sent on its way, and potentially harmful traffic, like attempts to gain control of a system through administrative requests, they get blocked before they ever touch the target. You can't hack what you can't touch. 

Now, this sounds like great security, but it doesn't answer all of the network control issues. Because a network ACL only gets to evaluate a packet if it crosses a subnet boundary, in or out. It doesn't evaluate if a packet can reach a specific EC2 instance or not. Sometimes, you'll have multiple EC2 instances in the same subnet, but they might have different rules around who can send them messages, what port those messages are allowed to be sent to. So you need instance level network security as well. 

To solve instance level access questions, we introduce security groups. Every EC2 instance, when it's launched, automatically comes with a security group. And by default, the security group does not allow any traffic into the instance at all. All ports are blocked; all IP addresses sending packets are blocked. That's very secure, but perhaps not very useful. If you want an instance to actually accept traffic from the outside, like say, a message from a front end instance or a message from the Internet. So obviously, you can modify the security group to accept a specific type of traffic. In the case of a website, you want web-based traffic or HTTPS to be accepted but not other types of traffic, say an operating system or administration requests. 

If NACLs are a passport control, a security group is like the doorman at your building, the building being the EC2 Instance, in this case. The doorman will check a list to ensure that someone is allowed to enter the building but won't bother check the list on the way out. With security groups, you allow specific traffic in and by default, all traffic is allowed out. Now, wait a minute, Blaine. You just described two different engines each doing the exact same job. Let good packets in, keep bad packets out. The key difference between a security group and a network ACL is the security group is stateful, meaning, as we talked about, it has some kind of a memory when it comes to who to allow in or out, and the network ACL is stateless, which remembers nothing and checks every single packet that crosses its border regardless of any circumstances. 

You know, this metaphor is important to understand. So I wanna illustrate the round trip of a packet as it goes from one instance to another instance in a different subnet. Now, this traffic management, it doesn't care about the contents of the packet itself. In fact, it doesn't even open the envelope, it can't. All it can do is check to see if the sender is on the approved list. 
All right. Let's start with instance A. We wanna send a packet from instance A to instance B in a different subnet, same VPC, different subnets. So instance A sends the packet. Now, the first thing that happens is that packet meets the boundary of the security group of instance A. By default, all outbound traffic is allowed from a security group. So you can walk right by the doormen and leave, cool, right. The packet made it past the security group of instance A. Now it has to leave the subnet boundary. At the boundary, the passport must now make it through passport control, the network ACL. The network ACL doesn't care about what the security group allowed. It has its own list of who can pass and who can't. If the target address is allowed, you can keep going on your journey, which it is. 

So now we have exited the original subnet and now the packet goes to the target subnet where instance B lives. Now at this target subnet, once again, we have passport control. Just because the packet was allowed out of its home country does not mean that it can enter the destination country or subnet in this case. They both have unique passport officers with their own checklists. You have to be approved on both lists, or you could get turned away at the border. Well, turns out the packet is on the approved list for this subnet, so it's allowed to enter through the network ACL into the subnet. Almost there. Now, we're approaching the target instance, instance B. Every EC2 Instance has their own security group. You wanna enter this instance, the doorman will need to check to see if you're allowed in, and we're on the list. The packet has reached the target instance. 

Once the transaction's complete, now it's just time to come home. It's the return traffic pattern. It's the most interesting, because this is where the stateful versus stateless nature of the different engines come into play. Because the packet still has to be evaluated at each checkpoint. Security groups, by default, allow all return traffic. So they don't have to check a list to see if they're allowed out. Instead, they automatically allow the return traffic to pass by no matter what. Passport control here at the subnet boundary, these network ACLs do not remember state. They don't care that the packet came in here. It might be on a you-can't-leave list. Every ingress and egress is checked with the appropriate list. The package return address has to be on their approved list to make it across the border. Made it to the border of the origin subnet, but we have to negotiate passport network ACL control here as well. Stateless controls, means it always checks its list. 

The packet pass the network ACL, the subnet level, which means the packets now made it back to instance A but the security group, the doorman is still in charge here. The key difference though is these security groups are stateful. The security group recognizes the packet from before. So it doesn't need to check to see if it's allowed in. Come on home. 

Now, this may seem like we spent a lot of effort just getting a packet from one instance to another and back. You might be concerned about all the network overhead this might generate. The reality is all of these exchanges happen instantly as part of how AWS Networking actually works. If you wanna know the technology that makes all that possible, well, then you need to sign up for a completely different set of trainings. Good network security will take advantage of both network ACLs and security groups, because security in-depth is critical for modern architectures.
