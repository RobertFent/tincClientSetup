# tinc setup on client side (rpi)

Client setup on arm architecture for connecting to tinc vpn.<br>
Based on: https://github.com/vimagick/dockerfiles/tree/master/tinc.<br>
I also use the image above on my server and let the client tinc folder generate there.

## setup

- extract tar file for client from server
- remove subnet 0.0.0.0 from tinc/netname/hosts/server (so dns won't break on rpi)
- run 'docker-compose-up -d'

## changes to make ssh into client work from server side

- used docker-compose w/ services
- added network_mode: host
- make sure ipv4 forwarding is enabled 'sudo sysctl ipv4.forward' should be 1
- run 'sudo iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT'
- run 'sudo iptables -A FORWARD -i tun0 -j ACCEPT'

## useful commands to check if everything worked

```$ ip route```
should generate the following output:
```bash
10.20.30.0/24 dev tun0 proto kernel scope link src 10.20.30.2
128.0.0.0/1 dev tun0 scope link
```

## performance testing

- on server side run 'iperf3 -s'
- on client side run 'iperf3 -c 10.20.30.1 -P 10 -R'
