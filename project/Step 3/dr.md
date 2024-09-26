# Infrastructure

## AWS Zones

Zone 1 : us-east-2

Zone 2 : us-west-1

## Servers and Clusters

### Table 1.1 Summary

| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| VPC               | Virtual Network       | -           |   2   | 2 zones, 1 VPC each zone for DR purpose                 |
| EC2 instances     | Application Servers   | t3.micro    |   6   | 2 zones, 3 instances each zone for DR purpose           |
| EC2 keypair       | SSH key for access EC2| -           |   2   | 2 zones, 1 keypair each zone for DR purpose             |
| EKS               | Kubernetes Clusters   | t3.medium   |   4   | 2 zones, 2 nodes each zone for DR purpose               |
| Prometheus Grafana| Monitoring System     | -           |   2   | 2 zones, 2 nodes each zone for DR purpose               |
| S3 bucket         | Bucket for terraform  | -           |   2   | 2 zones, 1 bucket each zone for DR purpose              |
| ALB               | App Load Balancer     | -           |   2   | 2 zones, 1 ALB for HA/DR purposes purpose                                       |
| RDS               | Backend Database      | db.t2.small |   2   | 1 cluster with 2 nodes in Zone 1, replicated to Zone 2 for DR purposes                              |
| RDS               | Backend Database      | db.t2.small |   2   | 1 cluster with 2 nodes, replication from Zone 1 to Zone 2       |

### Descriptions

More detailed descriptions of each asset identified above.

- VPC:  Virtual Private Cloud - Networking for the cloud infrastructure in both zones.
- EC2 instances:  Virtual Machines that run the application, with instances in both regions for redundancy and DR.
- EC2 keypair:  SSH keys for secure access to EC2 instances, one keypair per region for access to the instances.
- EKS: Kubernetes cluster used for running applications, including Prometheus and Grafana for monitoring.
- Prometheus Grafana: Monitoring stack to observe infrastructure and application metrics, with nodes deployed in both regions for DR purposes.
- S3 bucket: Used to store Terraform state, one bucket per region for DR replication of state files.
- ALB: Application Load Balancer for distributing traffic to EC2 instances in a highly available configuration across both regions.
- RDS: Relational Database Service (SQL database), primary cluster located in Zone 1 and replicated to Zone 2 for DR purposes.

## DR Plan

### Pre-Steps

- Ensure DR site setting and working properly

## Steps

Steps to perform in case of failover to the DR site:

- Promote RDS Cluster in DR Zone (Zone 2): Promote the replicated RDS cluster in Zone 2 to the primary database.
- Reroute Traffic: Update DNS or Application Load Balancer (ALB) settings to reroute traffic to the EC2 instances and services running in Zone 2.
- Monitoring: Ensure that Prometheus and Grafana are correctly monitoring the infrastructure in the DR site.
- Test Application: Perform testing of the application in the DR site to ensure that all services are working correctly and as expected.
