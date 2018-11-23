# SSH Config to proxy over a bastion

```
host bastion
    hostname <IP>
    ForwardAgent yes

host squid
    hostname <IP>
    user     ec2-user
    ProxyCommand ssh bastion -W [%h]:%p
```
