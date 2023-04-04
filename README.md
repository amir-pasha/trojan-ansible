# Setup Trojan Proxy with Ansible
This is an effort to automate setting up trojan proxy server with nginx.

## Requirements
- VPS running Ubuntu
- a registered domain pointing to your VPS server

### Set server ip address in ansible `inventory.ini` file

```
[webserver]
host ansible_host=1.2.3.4 ansible_user=root
```

### Set your desired variables

`variables/ssh.yaml`
```yaml
---
ssh_user: "user"
ssh_password: "$6$UJsEuR4OCLBtxL5H$f4VRQVt5WVcGIpJ1ExEJ6SvgT3Cjs3UI.cqQroJMLFXJY9ePuons6ic.eLe7oICF1OL79laX1UrUBjIN0GJ/Y."
ssh_port: "1234"
public_key_path: "/home/user/.ssh/id_rsa.pub" # local public key path on your machine
```

ssl certificate specific variables in `certificate.yaml`
```yaml
email: "account@mail.com"
primary_domain: "example.com" 
sub_domain: "sub.example.com" # trojan requests will be proxied to this domain
```

trojan variables in `variables/trojan.yaml`
```yaml
---
trojan_password: "xyz"
```
### 
It is recommended to perform these steps (change default ssh port, user, etc.) to avoid 
potential cyberattacks to your server, though it is not necessary.

```shell
ansible-playbook 01-add-ssh-user.yaml
```

```shell
ansible-playbook 02-generate-certificate.yaml --ask-become-pass
```

```shell
ansible-playbook 03-setup-nginx.yaml --ask-become-pass
```

```shell
ansible-playbook 04-setup-trojan.yaml --ask-become-pass
```
