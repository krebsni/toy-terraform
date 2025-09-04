# Following Hashicorp Terraform [AWS Get Started](https://developer.hashicorp.com/terraform/tutorials/aws-get-started)

## Install (on mac)

1. Install [Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

terraform -help
```

2. Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
On mac:

```bash
brew install awscli
aws --version
```

## Usage

1. Create AWS account and user with programmatic access and attach, e.g. `AdministratorAccess` policy.
2. Configure AWS CLI with your credentials

```bash
export AWS_ACCESS_KEY_ID="your_access_key_id"
export AWS_SECRET_ACCESS_KEY="your_secret_access_key"
```

3. Initialize and validate Terraform and create infrastructure

```bash
terraform init
terraform validate
```

4. Create infrastructure

```bash
terraform apply
```

5. Inspect state

```bash
terraform state list
terraform show
terraform output
```

6. Destroy infrastructure

```bash
terraform destroy
```

## Resources

- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform AWS VPC Module](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest)

## Additional Info

### AWS Subnets – Quick Summary

#### High-Level

- A **VPC** is your private network in AWS.
- A **subnet** is a slice of the VPC, bound to one Availability Zone.
- Subnets organize resources into **public** (internet-facing) and **private** (internal-only).

#### Subnet Types

- **Public subnet** → route to **Internet Gateway (IGW)**, resources can get public IPs.
- **Private subnet** → no direct internet access, outbound via **NAT Gateway**.
- **Isolated subnet** → no internet at all.

#### CIDR Blocks

- Subnets use **CIDR notation** (e.g. `10.0.1.0/24` = 256 IPs, 251 usable).
- AWS reserves 5 IPs in each subnet.
- Each subnet must fit inside the VPC’s CIDR block and not overlap with others.

#### IP Address Ranges

- Use **private IP ranges (RFC 1918)**:
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- These addresses are **internal only**, not routable on the public internet.
- To reach the internet, AWS maps a **public/Elastic IP** to the instance via IGW or Load Balancer.

#### Key Points

- Subnets are AZ-specific (one subnet = one AZ).
- Public subnets connect directly to IGW.
- Private subnets connect out via NAT but aren’t reachable from the internet.
- External users never see your `10.x.x.x` addresses — they see the mapped public IPs.
