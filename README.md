# AWS CloudFormation for Deploying Jitsi Meet on EC2

Deployment of [Jitsi Meet](https://jitsi.org/jitsi-meet/) as single node at an AWS EC2 instance.

The motivation of the project is to fastly generate a short-lived Jitsi Meet self hosted server, and easily dispose it later.

## Installation
In order to install and setup the self hosted Jitsi server, the official instructions are being followed: 
[Jitsi Meet quick install](https://github.com/jitsi/jitsi-meet/blob/master/doc/quick-install.md)

## Usage

### No Domain
The basic scenario is to install and setup a Jitsi server on an EC2 Instance, and access it by using the PublicDNS name of the generated Instance.

```bash
aws cloudformation deploy --template-file infrastructure.yml --stack-name ec2-jitsi-no-domain
```

##### Note:
Since a self signed certificate will be created for the PublicDNS name of the instance, a browser certificate warning/alert pops up when calling the URL.
The certificate should be accepted to proceed.


### With a provided Domain managed by AWS Route53
In this scenario, there is a domain managed by AWS Route53 service, and a record set will be created to forward the domain requests to the EC2 Instance.

```bash
aws cloudformation deploy --template-file infrastructure.yml --stack-name ec2-jitsi-route53 --parameter-overrides JitsiDomain=MY_JITSI_DOMAIN.NET Route53HostedZoneID=HOSTED_ZONE_ID
```

##### Note:
Since a self signed certificate will be created for the PublicDNS name of the instance, a browser certificate warning/alert pops up when calling the URL.
The certificate should be accepted to proceed.


### With Domain managed by AWS Route53 and Let's Encypt certificate
In this scenario, a certificate will be created as well to avoid the browser issue.

```bash
aws cloudformation deploy --template-file infrastructure.yml --stack-name ec2-jitsi-route53-cert --parameter-overrides JitsiDomain=MY_JITSI_DOMAIN.NET Route53HostedZoneID=HOSTED_ZONE_ID LetsEncryptEmail=EMAIL
```

##### Note:
Current solution is a bit ugly :(

Installation log can be found in the EC2 instance at `/home/ubuntu/progress.log` file.