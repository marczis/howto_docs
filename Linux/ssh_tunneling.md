# SSH Tunneling

How to use SSH to create a p2p connection between the target and your location

## Steps

 - Create tunnel device on your side
 - Create tunnel device on server side
 - Enable tunneling on the server (restart server)
 - Connect

## Create tunnel on both side

```bash
sudo ip tuntap add tun0 mode tun user ${USER}
```

## Setup the SSH server

```
PermitTunnel yes
```

## Connect

```
ssh -w 0:0 <target>
```
