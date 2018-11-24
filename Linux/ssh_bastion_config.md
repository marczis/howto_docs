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

So after you will be able to ssh to the "squid" target "directly" from your actual place.
Like 
```
ssh squid
```

Some notes:
 - You have to have ssh keys in place for the bastion and for the squid, they don't have to use the same, and you don't have to copy your keys anywhere it is using the ForwardingAgent - so you MUST trust in the bastion server otherwise don't do this. (Generally keep yourself away from server you don't trust :D)
 - Same setup will make "scp" magically work directly to the target (squid in this example) -> cool hah? 
 - Data will still go tru the bastion, so it won't be magically faster.