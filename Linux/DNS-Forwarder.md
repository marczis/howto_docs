# DNS Forwarder for VPC

Run two AWS Linux instancess with this user-data

```
#!/bin/bash
# Set the variables for your environment
eval $(ipcalc -n $(ip -o -4 a l eth0 | sed 's/.*inet \([^ ]*\).*/\1/'))
first=$(echo ${NETWORK} | sed 's/\([^\.]*.[^\.]*.[^\.]*\.\).*/\1/')
second=$(($(echo ${NETWORK} | sed 's/.*\.\(.*\)$/\1/') + 2))
vpc_dns=${first}${second}

onprem_domain=<DOMAIN TO FORWARD>
onprem_dns=<DNS SERVER>

# Install updates and dependencies
export http_proxy="http://XXX.XXX.XXX.XXX:PPPP"
export https_proxy="${http_proxy}"
yum update -y
yum install -y unbound

# Write Unbound configuration file with values from variables
cat << EOF > /etc/unbound/unbound.conf
server:
        interface: 0.0.0.0
        access-control: 0.0.0.0/0 allow
forward-zone:
        name: "."
        forward-addr: ${vpc_dns}
forward-zone:
        name: "${onprem_domain}"
        forward-addr: ${onprem_dns}
EOF

systemctl enable unbound
systemctl start unbound

```