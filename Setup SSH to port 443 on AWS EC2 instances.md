## Introduction

**Objectives:**

- The simplest solution over the more secure.
- Create EC2 instances accessible from Office LAN only without a VPN.

For better security, use a VPN or Direct Connect.

## Configuration

### VPC

- Set DNS hostnames and DNS resolution to yes 
in order for EC2 instances to resolve their auto-assigned internal hostnames correctly. This will avoid `sudo: unable to resolve host` error when you run command with `sudo`.
    ```
    DNS hostnames: yes
    DNS resolution: yes
    ```
- Attach an Internet Gateway to your VPC.

### Subnet

- Add the Internet Gateway to the subnet route table. Set the destination to `0.0.0.0/0`. 

### Security Group

Port 22 is usually used for SSH. However, it may also be blocked by office router. To bypass this limitation you need to setup SSH to listen another port. Port 443 is used for HTTPS and is always open. It must be open at security group level.

- Add inbound rule to allow traffic from office LAN external IP address (`xxx.xxx.xxx.xxx/32`) on port 443.

### SSH

- Update SSH Daemon configuration file `/etc/ssh/sshd_config`. Change `Port 22` with:
    ```
    Port 443
    ```
- Restart SSH daemon:
    ```sh
    sudo service sshd restart
    ```
- Connect from your office workstations:
    ```sh
    ssh -i <aws-certificate.pem> -p 443 user@ec2-instance-ip
    ```
