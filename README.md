# EKS-cluster-using-Terraform

### Provisionig Kubernetes Cluster in AWS using managed Kubernetes service EKS using Terraform.

```bash
figlet
  _____ _  ______     ____ _           _              _   _     _             
 | ____| |/ / ___|   / ___| |_   _ ___| |_ ___ _ __  | | | |___(_)_ __   __ _ 
 |  _| | ' /\___ \  | |   | | | | / __| __/ _ \ '__| | | | / __| | '_ \ / _` |
 | |___| . \ ___) | | |___| | |_| \__ \ ||  __/ |    | |_| \__ \ | | | | (_| |
 |_____|_|\_\____/   \____|_|\__,_|___/\__\___|_|     \___/|___/_|_| |_|\__, |
                                                                        |___/ 
                _____                    __                      
               |_   _|__ _ __ _ __ __ _ / _| ___  _ __ _ __ ___  
                 | |/ _ \ '__| '__/ _` | |_ / _ \| '__| '_ ` _ \ 
                 | |  __/ |  | | | (_| |  _| (_) | |  | | | | | |
                 |_|\___|_|  |_|  \__,_|_|  \___/|_|  |_| |_| |_|
                                                                 
```
This takes about 15 min to provision the Kubernetes on EKS

### List of Resources created: Total 18
- VPC (1)
- Public Subnets (2)
- Route Table (1)
- Associations (2) with each public subnet
- EKS Cluster (1) Master Node managed my AWS
    - cluster_name = "terraform-eks-demo"
- Node Group (1)
    - node_group_name = "demo"
- Worker Node (1)
- Security Groups (1)
- Rule (1) attached to the security group
- IAM Roles (2) one for Master and one for Worker
    - terraform-eks-demo-cluster
    - terraform-eks-demo-node
- Policies (5) - 2 for ther role attached to master and 3 for the role attached to Worker.
    - AmazonEKSClusterPolicy
    - AmazonEKSVPCResourceController
    - AmazonEKSWorkerNodePolicy
    - AmazonEKS_CNI_Policy
    - AmazonEC2ContainerRegistryReadOnly

### Pre-requisites
- AWS CLI (installed and configured with aws configure)
- Kubectl
- Eksctl


```bash
git clone https://github.com/hashicorp/terraform-provider-aws.git
cd terraform-provider-aws
cd examples
cd eks-getting-started
terraform init
terraform plan
terraform apply
terraform destroy
```

### Output of 'terraform plan'
```bash
# aws_eks_cluster.demo will be created
  + resource "aws_eks_cluster" "demo" {
      + arn                   = (known after apply)
      + certificate_authority = (known after apply)
      + cluster_id            = (known after apply)
      + created_at            = (known after apply)
      + endpoint              = (known after apply)
      + id                    = (known after apply)
      + identity              = (known after apply)
      + name                  = "terraform-eks-demo"
      + platform_version      = (known after apply)
      + role_arn              = (known after apply)
      + status                = (known after apply)
      + tags_all              = (known after apply)
      + version               = (known after apply)

      + vpc_config {
          + cluster_security_group_id = (known after apply)
          + endpoint_private_access   = false
          + endpoint_public_access    = true
          + public_access_cidrs       = (known after apply)
          + security_group_ids        = (known after apply)
          + subnet_ids                = (known after apply)
          + vpc_id                    = (known after apply)
        }
    }

  # aws_eks_node_group.demo will be created
  + resource "aws_eks_node_group" "demo" {
      + ami_type               = (known after apply)
      + arn                    = (known after apply)
      + capacity_type          = (known after apply)
      + cluster_name           = "terraform-eks-demo"
      + disk_size              = (known after apply)
      + id                     = (known after apply)
      + instance_types         = (known after apply)
      + node_group_name        = "demo"
      + node_group_name_prefix = (known after apply)
      + node_role_arn          = (known after apply)
      + release_version        = (known after apply)
      + resources              = (known after apply)
      + status                 = (known after apply)
      + subnet_ids             = (known after apply)
      + tags_all               = (known after apply)
      + version                = (known after apply)

      + scaling_config {
          + desired_size = 1
          + max_size     = 1
          + min_size     = 1
        }
    }

  # aws_iam_role.demo-cluster will be created
  + resource "aws_iam_role" "demo-cluster" {
      + arn                   = (known after apply)
      + assume_role_policy    = jsonencode(
            {
              + Statement = [
                  + {
                      + Action    = "sts:AssumeRole"
                      + Effect    = "Allow"
                      + Principal = {
                          + Service = "eks.amazonaws.com"
                        }
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + create_date           = (known after apply)
      + force_detach_policies = false
      + id                    = (known after apply)
      + managed_policy_arns   = (known after apply)
      + max_session_duration  = 3600
      + name                  = "terraform-eks-demo-cluster"
      + name_prefix           = (known after apply)
      + path                  = "/"
      + tags_all              = (known after apply)
      + unique_id             = (known after apply)
    }

  # aws_iam_role.demo-node will be created
  + resource "aws_iam_role" "demo-node" {
      + arn                   = (known after apply)
      + assume_role_policy    = jsonencode(
            {
              + Statement = [
                  + {
                      + Action    = "sts:AssumeRole"
                      + Effect    = "Allow"
                      + Principal = {
                          + Service = "ec2.amazonaws.com"
                        }
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + create_date           = (known after apply)
      + force_detach_policies = false
      + id                    = (known after apply)
      + managed_policy_arns   = (known after apply)
      + max_session_duration  = 3600
      + name                  = "terraform-eks-demo-node"
      + name_prefix           = (known after apply)
      + path                  = "/"
      + tags_all              = (known after apply)
      + unique_id             = (known after apply)
    }

  # aws_iam_role_policy_attachment.demo-cluster-AmazonEKSClusterPolicy will be created
  + resource "aws_iam_role_policy_attachment" "demo-cluster-AmazonEKSClusterPolicy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
      + role       = "terraform-eks-demo-cluster"
    }

  # aws_iam_role_policy_attachment.demo-cluster-AmazonEKSVPCResourceController will be created
  + resource "aws_iam_role_policy_attachment" "demo-cluster-AmazonEKSVPCResourceController" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKSVPCResourceController"
      + role       = "terraform-eks-demo-cluster"
    }

  # aws_iam_role_policy_attachment.demo-node-AmazonEC2ContainerRegistryReadOnly will be created
  + resource "aws_iam_role_policy_attachment" "demo-node-AmazonEC2ContainerRegistryReadOnly" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
      + role       = "terraform-eks-demo-node"
    }

  # aws_iam_role_policy_attachment.demo-node-AmazonEKSWorkerNodePolicy will be created
  + resource "aws_iam_role_policy_attachment" "demo-node-AmazonEKSWorkerNodePolicy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
      + role       = "terraform-eks-demo-node"
    }

  # aws_iam_role_policy_attachment.demo-node-AmazonEKS_CNI_Policy will be created
  + resource "aws_iam_role_policy_attachment" "demo-node-AmazonEKS_CNI_Policy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
      + role       = "terraform-eks-demo-node"
    }

  # aws_internet_gateway.demo will be created
  + resource "aws_internet_gateway" "demo" {
      + arn      = (known after apply)
      + id       = (known after apply)
      + owner_id = (known after apply)
      + tags     = {
          + "Name" = "terraform-eks-demo"
        }
      + tags_all = {
          + "Name" = "terraform-eks-demo"
        }
      + vpc_id   = (known after apply)
    }

  # aws_route_table.demo will be created
  + resource "aws_route_table" "demo" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + owner_id         = (known after apply)
      + propagating_vgws = (known after apply)
      + route            = [
          + {
              + carrier_gateway_id         = ""
              + cidr_block                 = "0.0.0.0/0"
              + core_network_arn           = ""
              + destination_prefix_list_id = ""
              + egress_only_gateway_id     = ""
              + gateway_id                 = (known after apply)
              + ipv6_cidr_block            = ""
              + local_gateway_id           = ""
              + nat_gateway_id             = ""
              + network_interface_id       = ""
              + transit_gateway_id         = ""
              + vpc_endpoint_id            = ""
              + vpc_peering_connection_id  = ""
            },
        ]
      + tags_all         = (known after apply)
      + vpc_id           = (known after apply)
    }

  # aws_route_table_association.demo[0] will be created
  + resource "aws_route_table_association" "demo" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

  # aws_route_table_association.demo[1] will be created
  + resource "aws_route_table_association" "demo" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

  # aws_security_group.demo-cluster will be created
  + resource "aws_security_group" "demo-cluster" {
      + arn                    = (known after apply)
      + description            = "Cluster communication with worker nodes"
      + egress                 = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = ""
              + from_port        = 0
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "-1"
              + security_groups  = []
              + self             = false
              + to_port          = 0
            },
        ]
      + id                     = (known after apply)
      + ingress                = (known after apply)
      + name                   = "terraform-eks-demo-cluster"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags                   = {
          + "Name" = "terraform-eks-demo"
        }
      + tags_all               = {
          + "Name" = "terraform-eks-demo"
        }
      + vpc_id                 = (known after apply)
    }

  # aws_security_group_rule.demo-cluster-ingress-workstation-https will be created
  + resource "aws_security_group_rule" "demo-cluster-ingress-workstation-https" {
      + cidr_blocks              = [
          + "84.247.43.120/32",
        ]
      + description              = "Allow workstation to communicate with the cluster API Server"
      + from_port                = 443
      + id                       = (known after apply)
      + protocol                 = "tcp"
      + security_group_id        = (known after apply)
      + security_group_rule_id   = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 443
      + type                     = "ingress"
    }

  # aws_subnet.demo[0] will be created
  + resource "aws_subnet" "demo" {
      + arn                                            = (known after apply)
      + assign_ipv6_address_on_creation                = false
      + availability_zone                              = "us-east-1a"
      + availability_zone_id                           = (known after apply)
      + cidr_block                                     = "10.0.0.0/24"
      + enable_dns64                                   = false
      + enable_resource_name_dns_a_record_on_launch    = false
      + enable_resource_name_dns_aaaa_record_on_launch = false
      + id                                             = (known after apply)
      + ipv6_cidr_block_association_id                 = (known after apply)
      + ipv6_native                                    = false
      + map_public_ip_on_launch                        = true
      + owner_id                                       = (known after apply)
      + private_dns_hostname_type_on_launch            = (known after apply)
      + tags                                           = {
          + "Name"                                     = "terraform-eks-demo-node"
          + "kubernetes.io/cluster/terraform-eks-demo" = "shared"
        }
      + tags_all                                       = {
          + "Name"                                     = "terraform-eks-demo-node"
          + "kubernetes.io/cluster/terraform-eks-demo" = "shared"
        }
      + vpc_id                                         = (known after apply)
    }

  # aws_subnet.demo[1] will be created
  + resource "aws_subnet" "demo" {
      + arn                                            = (known after apply)
      + assign_ipv6_address_on_creation                = false
      + availability_zone                              = "us-east-1b"
      + availability_zone_id                           = (known after apply)
      + cidr_block                                     = "10.0.1.0/24"
      + enable_dns64                                   = false
      + enable_resource_name_dns_a_record_on_launch    = false
      + enable_resource_name_dns_aaaa_record_on_launch = false
      + id                                             = (known after apply)
      + ipv6_cidr_block_association_id                 = (known after apply)
      + ipv6_native                                    = false
      + map_public_ip_on_launch                        = true
      + owner_id                                       = (known after apply)
      + private_dns_hostname_type_on_launch            = (known after apply)
      + tags                                           = {
          + "Name"                                     = "terraform-eks-demo-node"
          + "kubernetes.io/cluster/terraform-eks-demo" = "shared"
        }
      + tags_all                                       = {
          + "Name"                                     = "terraform-eks-demo-node"
          + "kubernetes.io/cluster/terraform-eks-demo" = "shared"
        }
      + vpc_id                                         = (known after apply)
    }

  # aws_vpc.demo will be created
  + resource "aws_vpc" "demo" {
      + arn                                  = (known after apply)
      + cidr_block                           = "10.0.0.0/16"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = (known after apply)
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + tags                                 = {
          + "Name"                                     = "terraform-eks-demo-node"
          + "kubernetes.io/cluster/terraform-eks-demo" = "shared"
        }
      + tags_all                             = {
          + "Name"                                     = "terraform-eks-demo-node"
          + "kubernetes.io/cluster/terraform-eks-demo" = "shared"
        }
    }

Plan: 18 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + config_map_aws_auth = (known after apply)
  + kubeconfig          = (known after apply)
```

### Total of 18 Resources will be created.
