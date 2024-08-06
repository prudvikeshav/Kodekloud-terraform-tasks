# Terraform Challenge Series

Welcome to the Terraform Challenge Series! This README provides a concise overview of the three challenges, each with its own problem statement and solutions.

## Table of Contents

1. [Challenges Overview](#challenges-overview)
2. [Challenges Details](#challenges-details)
   - [Challenge Lab 1](#challenge-lab-1)
   - [Challenge Lab 2](#challenge-lab-2)
   - [Challenge Lab 3](#challenge-lab-3)
3. [Getting Started](#getting-started)
4. [Resources](#resources)
5. [Support](#support)

## Challenges Overview

This series includes three Terraform labs, each addressing different infrastructure scenarios:

1. **Challenge Lab 1**: Kubernetes Deployment.
2. **Challenge Lab 2**: Deploying an EC2 instance with IAM role.
3. **Challenge Lab 3**: Setting up an RDS instance and configuring security groups.

## Challenges Details

### Challenge Lab 1

**Problem Statement:** Set up a VPC with two public and two private subnets, and a security group allowing SSH access.

**Solution Folder:** [lab1](./lab1)

### Challenge Lab 2

**Problem Statement:** Deploy an EC2 instance in a private subnet with a key pair and IAM role for S3 access.

**Solution Folder:** [lab2](./lab2)

### Challenge Lab 3

**Problem Statement:** Create an RDS instance in a private subnet with an automatic backup policy and appropriate security group rules.

**Solution Folder:** [lab3](./lab3)

## Getting Started

1. **Clone the Repository:**

   ```sh
   git clone https://github.com/your-repo/terraform-challenges.git
   cd terraform-challenges
   ```

2. **Navigate to Each Lab:**

   ```sh
   cd lab1
   terraform init
   terraform apply

   cd ../lab2
   terraform init
   terraform apply

   cd ../lab3
   terraform init
   terraform apply
   ```

## Resources

- [Terraform Documentation](https://www.terraform.io/docs)
- [AWS Documentation](https://docs.aws.amazon.com/)

## Support

For support, please refer to the [Terraform Community](https://discuss.hashicorp.com/c/terraform) or reach out on related forums.

Happy Terraforming! 🚀
