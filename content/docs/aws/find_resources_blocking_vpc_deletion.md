---
title: "Find resources blocking VPC deletion" 
---


```bash
#!/bin/bash
vpc="$1"
region="eu-west-2"

echo "Checking IGWs" 
aws ec2 describe-internet-gateways --region $region --filters 'Name=attachment.vpc-id,Values='"$vpc" | grep InternetGatewayId
printf "\n\n" 

echo "Checking subnets"
aws ec2 describe-subnets --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep SubnetId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking route tables"
aws ec2 describe-route-tables --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep RouteTableId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking network acls"
aws ec2 describe-network-acls --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep NetworkAclId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking vpc peering connections"
aws ec2 describe-vpc-peering-connections --region $region --filters 'Name=requester-vpc-info.vpc-id,Values='"$vpc" | grep VpcPeeringConnectionId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking vpc endpoints"
aws ec2 describe-vpc-endpoints --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep VpcEndpointId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking nat gateways"
aws ec2 describe-nat-gateways --region $region --filter 'Name=vpc-id,Values='"$vpc" | grep NatGatewayId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking security groups"
aws ec2 describe-security-groups --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep GroupId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking ec2 instances"
aws ec2 describe-instances --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep InstanceId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking vpn gateways"
aws ec2 describe-vpn-gateways --region $region --filters 'Name=attachment.vpc-id,Values='"$vpc" | grep VpnGatewayId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking network interfaces"
aws ec2 describe-network-interfaces --region $region --filters 'Name=vpc-id,Values='"$vpc" | grep NetworkInterfaceId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking carrier gateways"
aws ec2 describe-carrier-gateways --region $region --filters Name=vpc-id,Values="$vpc" | grep CarrierGatewayId | cut -d ":" -f 2
printf "\n\n" 

echo "Checking local gateway route table vpc associations"
aws ec2 describe-local-gateway-route-table-vpc-associations --region $region --filters Name=vpc-id,Values="$vpc" | grep LocalGatewayRouteTableVpcAssociationId | cut -d ":" -f 2
printf "\n\n"  
```
