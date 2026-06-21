#AWS VPC Public and Private Subnet Lab
##Overview

This project demonstrates a foundational AWS networking pattern: a custom VPC with both a public subnet and a private subnet.

The goal of this lab was to understand how AWS routes internet traffic for public and private resources using an Internet Gateway, NAT Gateway, and separate route tables.

##Architecture
Demo VPC: 10.0.0.0/16

                         Internet
                            |
                            v
                     Internet Gateway
                            |
                            v
              Public Subnet: 10.0.1.0/24
                            |
                            v
                        NAT Gateway
                            |
                            v
             Private Subnet: 10.0.2.0/24
##Resources Created
.Custom VPC: 10.0.0.0/16
.Public subnet: 10.0.1.0/24
.Private subnet: 10.0.2.0/24
.Internet Gateway
.NAT Gateway
.Public route table
.Private route table
.Route Table Design
.Public Route Table

The public route table allows internet-bound traffic to reach the Internet Gateway.

10.0.0.0/16 → local
0.0.0.0/0  → Internet Gateway

Associated subnet:

Demo Public Subnet
10.0.1.0/24
Private Route Table

The private route table allows private resources to reach the internet outbound through the NAT Gateway.

10.0.0.0/16 → local
0.0.0.0/0  → NAT Gateway

Associated subnet:

Demo Private Subnet
10.0.2.0/24
What This Lab Demonstrates

This lab shows the difference between public and private subnet routing in AWS.

A public subnet has a route directly to an Internet Gateway. Resources in this subnet can be internet-facing if they have a public IP address and the correct security group rules.

A private subnet does not route directly to the Internet Gateway. Instead, private resources route outbound traffic to a NAT Gateway, which is placed in the public subnet. This allows private resources to download updates or reach external services without being directly reachable from the internet.

Key Lessons Learned
A public subnet uses an Internet Gateway for direct internet access.
A private subnet uses a NAT Gateway for outbound IPv4 internet access.
A NAT Gateway must be placed in a public subnet.
Private subnet resources do not route directly to the Internet Gateway.
Route table associations determine whether a subnet behaves as public or private.
Egress-only Internet Gateway is for IPv6 outbound-only traffic, not IPv4.
Practical Use Case

This design is commonly used in production web applications.

Users
  ↓
Public Subnet
Application Load Balancer
  ↓
Private Subnet
Application servers
  ↓
Private Subnet
Database

Only the public-facing layer is exposed to the internet. Backend systems remain private while still having outbound internet access for patching, updates, or external API calls.

Security Notes
Private subnet resources should not receive public IP addresses.
Security groups should restrict inbound access based on application requirements.
NAT Gateway allows outbound traffic but does not allow inbound internet-initiated connections to private instances.
AWS account IDs, public IPs, and sensitive resource details should be removed from screenshots before publishing.
Cleanup Reminder

NAT Gateways and Elastic IPs can create ongoing AWS charges. After completing the lab, delete the NAT Gateway and release the Elastic IP if the environment is no longer needed.
