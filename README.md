# EKS Hybrid Node Demo

This project demonstrates how to set up an Amazon EKS cluster with hybrid nodes - allowing you to connect non-AWS nodes to your EKS cluster. This is particularly useful for hybrid/multi-cloud scenarios or when you need to connect on-premises nodes to your EKS cluster.

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.3.2
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- [Packer](https://developer.hashicorp.com/packer/tutorials/docker-get-started/get-started-install-cli) for building the custom AMI
- [kubectl](https://kubernetes.io/docs/tasks/tools/) for interacting with the cluster
- [Helm](https://helm.sh/docs/intro/install/) >= 2.16

## Architecture

This setup creates:
1. An EKS cluster in your primary region (eg. `us-west-2`)
2. A VPC in a secondary region for hybrid nodes (eg. `us-east-1`)
3. VPC peering between the two regions
4. Custom AMI for hybrid nodes using Packer
5. Two EC2 instances as demonstration hybrid nodes
6. Cilium CNI configuration for pod networking


## Important Notes

- This is a demonstration setup. AWS EC2 instances are not officially supported as EKS hybrid nodes.
- The example creates resources that may incur costs. Remember to clean up when done:

```bash
terraform destroy
```

- SSH access is configured to allow connections from anywhere (0.0.0.0/0) for demonstration purposes. In production, restrict this to specific IP ranges.

## Components

- `main.tf` - EKS cluster and primary VPC configuration
- `remote.tf` - Hybrid node infrastructure in secondary region
- `ami/` - Packer configuration for building hybrid node AMI
- `versions.tf` - Provider and version constraints
- `outputs.tf` - Useful output values including kubectl configuration command

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.3.2 |
| aws | >= 5.79 |
| helm | >= 2.16 |
| http | >= 3.4 |
| local | >= 2.5 |
| tls | >= 4.0 |

## Security Considerations

- The example uses AWS Systems Manager (SSM) for node authentication
- VPC peering is configured between cluster and hybrid node networks
- IMDS is disabled on hybrid nodes to make them appear more like non-AWS instances
- Cilium CNI is configured for secure pod networking

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License  

This project is licensed under the MIT License - see the LICENSE file for details.  