# Day 3 — Hour 2: VPC + Public/Private Subnets + Routing

## Objective
Create a custom VPC with one public subnet and one private subnet, and prove the difference using route tables (IGW route vs no IGW route).

## Build Summary
- Region: us-east-2
- VPC: joshua-vpc (10.10.0.0/16)
- Public Subnet: joshua-public-1 (10.10.1.0/24) in <AZ>
- Private Subnet: joshua-private-1 (10.10.2.0/24) in <AZ>
- Internet Gateway: joshua-igw attached to joshua-vpc
- Public Route Table: joshua-rt-public
  - Routes: 10.10.0.0/16 -> local
  - Routes: 0.0.0.0/0 -> joshua-igw
  - Subnet association: joshua-public-1
- Private Route Table: joshua-rt-private
  - Routes: 10.10.0.0/16 -> local
  - No 0.0.0.0/0 route to IGW
  - Subnet association: joshua-private-1

## Key Concept (What makes a subnet “public”)
A subnet is “public” when its route table has a default route (0.0.0.0/0) pointing to an Internet Gateway (IGW).  
A subnet is “private” when it does NOT have a default route to an IGW (it may later use NAT for outbound internet).

## Screenshots
- screenshots/day03-hour02-01-vpc.png
- screenshots/day03-hour02-02-subnets.png
- screenshots/day03-hour02-03-rt-public.png
- screenshots/day03-hour02-04-rt-private.png

## Notes / Lessons
- The subnet name doesn’t matter—route table association does.
- IGW must be attached to the VPC AND referenced in a route table to enable internet routing.
