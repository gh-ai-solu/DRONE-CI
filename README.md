# Docker Templates

This is a repository that targets [AWS ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html) and [CircleCI](https://circleci.com/). This objective is to push Docker images to a private repository where they can then be used in other cloud workflows (such as k8s, ECS, or Fargate).

## Install Docker CLI

### For WSL Users

`sudo apt install docker.io`

If there are still problems getting Docker to run in CLI this [blog](https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4) could be a tremendous help.

### General Windows Users

1. Download and install [Docker Desktop](https://hub.docker.com/editions/community/docker-ce-desktop-windows). You may have to create a free account

2. Enable Virtualization on your machine's BIOS. Follow instructions here, understanding that there may be differences in appearance between your machine and the article screenshots when you access the BIOS: [Virtualization](https://support.bluestacks.com/hc/en-us/articles/115003174386-How-can-I-enable-virtualization-VT-on-my-PC-)
3. Enable Hyper-V on your machine: [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

### For Powershell Users

1. Download and install [Docker Desktop](https://hub.docker.com/editions/community/docker-ce-desktop-windows). You may have to create a free account

2. Enable Virtualization on your machine's BIOS. Follow instructions here, understanding that there may be differences in appearance between your machine and the article screenshots when you access the BIOS: [Virtualization](https://support.bluestacks.com/hc/en-us/articles/115003174386-How-can-I-enable-virtualization-VT-on-my-PC-)
3. Enable Hyper-V on your machine: [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)
4. Copy and paste the following command in a Windows Powershell window: `& $Env:ProgramFiles\Docker\Docker\DockerCli.exe -SwitchDaemon .`

## How it Works

:Diagram:

## Getting Started

The overall setup is well described in this [blog post](https://circleci.com/blog/cd-for-pwa/). You'll need a CircleCI project, and [Context](https://circleci.com/docs/2.0/env-vars). You'll also need an IAM user's API credentials, with at least the following permissions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Action": [
                "ecr:*",
                "cloudtrail:LookupEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

Connect your project to the context and to your fork/clone of this repository. Populate the context with the following variables:

- AWS_ECR_ACCOUNT_URL (i.e. AWS_ACCOUNT_NUMBER.dkr.ecr.REGION.amazonaws.com)
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_REGION

These variables are used by the [AWS-ECR CircleCI Orb](https://circleci.com/orbs/registry/orb/circleci/aws-ecr), and you can consult the documentation for further options and customizations available.

## Motivation

[Mounting evidence](https://itrevolution.com/book/accelerate/) suggests that continous delivery and frequent deployment improves business outcomes. Docker and CI pipelines are critical tools which enable the practice of "continous deployment", where code is deployed automatically whenever it is checked in to version control.
