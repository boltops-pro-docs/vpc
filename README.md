<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/vpc/blob/master/README.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

# VPC Blueprint

[![Watch the video](https://img.boltops.com/boltopspro/video-preview/multiple/vpc.png)](https://www.youtube.com/watch?v=--PX08gdst8)

![CodeBuild](https://codebuild.us-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiaDByaE9HZEgrRFgxcHZhNXJIWnFpKzNwOHJJejRVVk9VVU00aW1XRFJtYzdhV0Z0L1hrTUp3cmdCTVpNWU5HL0daZTdPci9kem9mRE1XV3AvR3JCd1BVPSIsIml2UGFyYW1ldGVyU3BlYyI6IlBub2FjaVdMLzQ2dmVEOSsiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

[![BoltOps Badge](https://img.boltops.com/boltops/badges/boltops-badge.png)](https://www.boltops.com)

This blueprint provisions a highly available VPC based on the [Standardized Architecture for PCI DSS](https://docs.aws.amazon.com/quickstart/latest/compliance-pci/overview.html).

* By default, it's configured to provision Subnets across 3 Availability Zones. This can easily change to less or more AZs.
* In each AZ, it provisions a public subnet, private app subnet, and private data subnet. This can also easily be changed.

![](https://www.boltops.com/assets/faq/boltops-example-architecture-0a6fec7422d2caa0029c7fdfe3f8a62b3518591bf1a94e7b38849d46cd720daa.png)

## Prerequisite

* Setup config/settings.yml: [reference-architecture/docs/settings-setup.md](https://github.com/boltopspro-docs/reference-architecture/blob/master/docs/settings-setup.md)

## Usage

1. Add blueprint to your Gemfile
2. Configure configs/vpc values
3. Deploy blueprint

## Add

Add the blueprint to your lono project's `Gemfile`.

```ruby
gem "vpc", git: "git@github.com:boltopspro/vpc.git"
```

## Configure

Configure the `configs/vpc/params` and `configs/vpc/variables` files. We'll configure all 3 vpc blueprints:

    LONO_ENV=development lono seed vpc
    LONO_ENV=management  lono seed vpc
    LONO_ENV=production  lono seed vpc

The generated files in `configs/vpc` folder look something like this:

    configs/vpc/
    ├── params
    │   ├── development.txt
    │   ├── management.txt
    │   └── production.txt
    └── variables
        ├── development.rb
        ├── management.rb
        └── production.rb

So you have an idea of what they look like, here are the development ones:

configs/vpc/params/development.txt:

    DomainName=development.local
    VpcName=main-development

configs/vpc/variables/development.rb:

```ruby
@vpc_cidr = "10.20.0.0/16"
@subnets = [
  {name: "PrivateApp1",  cidr: "10.20.0.0/19"},   # 8,192 hosts
  {name: "PrivateData1", cidr: "10.20.32.0/20"},  # 4,096 hosts
  {name: "Public1",      cidr: "10.20.48.0/20"},  # 4,096 hosts

  {name: "PrivateApp2",  cidr: "10.20.64.0/19"},  # 8,192 hosts
  {name: "PrivateData2", cidr: "10.20.96.0/20"},  # 4,096 hosts
  {name: "Public2",      cidr: "10.20.112.0/20"}, # 4,096 hosts

  {name: "PrivateApp3",  cidr: "10.20.128.0/19"}, # 8,192 hosts
  {name: "PrivateData3", cidr: "10.20.160.0/20"}, # 4,096 hosts
  {name: "Public3",      cidr: "10.20.176.0/20"}, # 4,096 hosts
]
```

Check on the values and change them for your needs for all 3 environments: development, production, and management. Each environment VPC has different CIDR ranges so we can peer them later.

## Deploy

Let's deploy now with [lono cfn deploy](https://lono.cloud/reference/lono-cfn-deploy/).

    LONO_ENV=development lono cfn deploy vpc --sure --no-wait
    LONO_ENV=management  lono cfn deploy vpc --sure --no-wait
    LONO_ENV=production  lono cfn deploy vpc --sure --no-wait

And just like that we have 3 VPCs.

If you are using a Single AWS Account, use these commands instead: [One Account](docs/one-account.md).

## IAM Permissions

The IAM permissions required for this stack are described below.

Service | Description
--- | ---
cloudformation | To launch the CloudFormation stack.
ec2 | VPC components like Subnets, Route Tables, etc.
elasticache | ElastiCache CacheSubnetGroup
iam | IAM role to enable VPC Flow log.
rds | RDS DBSubnetGroup
s3 | Lono managed s3 bucket

## Back to Reference Architecture

That's it. Go back to the main [boltopspro/reference-architecture](https://github.com/boltopspro-docs/reference-architecture/blob/master/README.md)
