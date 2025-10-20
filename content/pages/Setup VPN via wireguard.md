---
tags:
- Tech
- WireGuard
date: 2023-01-31
title: Setup VPN via wireguard
categories:
lastMod: 2025-10-18
--- 
## Reference
  
[How To Set Up WireGuard on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04)
  
https://www.wireguardconfig.com/
  
[Secure Communication with a WireGuard VPN &middot; Ultimate Security Professional Blog](https://ultimatesecurity.pro/post/wireguard/#:~:text=Delete%20the%20WireGuard%20interface%20During%20your%20tests%20you,wg.%20Let%E2%80%99s%20execute%20once%20again%20the%20command%20wg.)
  
## TL;DR

```
# server.conf
[Interface]
Address = 10.6.0.1/24
PostUp = ufw route allow in on wg0 out on eth0
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PreDown = ufw route delete allow in on wg0 out on eth0
PreDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 51820
PrivateKey = <server generated private key>

[Peer]
PublicKey = <client1 generated public key>
AllowedIPs = 10.6.0.2/32

[Peer]
PublicKey = <client2 generated public key>
AllowedIPs = 10.6.0.3/32


# client1.conf
[Interface]
PrivateKey = <client1 generated private key>
Address = 10.6.0.2/24
DNS = 208.67.222.222

[Peer]
PublicKey = <server generated public key>
AllowedIPs = 0.0.0.0/0
Endpoint = <server_ip>:51820


# client2.conf
[Interface]
PrivateKey = <client2 generated private key>
Address = 10.6.0.3/24
DNS = 208.67.222.222

[Peer]
PublicKey = <server generated public key>
AllowedIPs = 0.0.0.0/0
Endpoint = <server_ip>:51820
```
  
Server side

  
  + Interface Address can be randomly picked

  
  + Peer AllowedIPs should be correspondent to client side, with mask `/32`
  
Client sides

  
  + Interface Address is in the subnet of server side interface address

  
  + Interface Addresses of different clients must not intersect

  
  + Interface Address has mask `/24`.
  
## Step 1 — Installing WireGuard and Generating a Key Pair

The first step in this tutorial is to install WireGuard on your 
server. To start off, update your WireGuard Server’s package index and 
install WireGuard using the following commands. You may be prompted to 
provide your sudo user’s password if this is the first time you’re using
`sudo` in this session:

```shell
sudo apt update
sudo apt install wireguard
```

Now that you have WireGuard installed, the next step is to generate a 
private and public keypair for the server. You’ll use the built-in `wg genkey` and `wg pubkey` commands to create the keys, and then add the private key to WireGuard’s configuration file.

You will also need to change the permissions on the key that you just created using the `chmod` command, since by default the file is readable by any user on your server.

Create the private key for WireGuard and change its permissions using the following commands:

```shell
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
```

The `sudo chmod go=...` command removes any permissions on the file for users and groups other than the root user to ensure that only it can access the private key.

You should receive a single line of `base64` encoded output, which is the private key. A copy of the output is also stored in the `/etc/wireguard/private.key` file for future reference by the `tee` portion of the command. Carefully make a note of the private key that is output since you’ll need to add it to WireGuard’s configuration file 
later in this section.

The next step is to create the corresponding public key, which is 
derived from the private key. Use the following command to create the 
public key file:

```shell
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

This command consists of three individual commands that are chained together using the `|` (pipe) operator:
  
`sudo cat /etc/wireguard/private.key`: this command reads the private key file and outputs it to the [standard output](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection#standard-output) stream.
  
`wg pubkey`: the second command takes the output from the first command as its [standard input](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection#standard-input) and processes it to generate a public key.
  
`sudo tee /etc/wireguard/public.key`: the final command takes the output of the public key generation command and redirects it into the file named `/etc/wireguard/public.key`.

When you run the command you will again receive a single line of `base64` encoded output, which is the public key for your WireGuard Server. Copy it somewhere for reference, since you will need to distribute the public key to any peer that connects to the server.
  
### Step 2(a) — Choosing an IPv4 Range

10.8.0.1/24 for example
  
### Step 2(b) — Choosing an IPv6 Range

This server doesn't have an ipv6 address.

Generate method: [RFC 4193](https://www.rfc-editor.org/rfc/rfc4193#section-3)
  
## Step 3 — Creating a WireGuard Server Configuration

```shell
sudo vim /etc/wireguard/wg0.conf
```

Add the following lines to the file, substituting your private key in place of the highlighted `base64_encoded_private_key_goes_here` value, and the IP address(es) on the `Address` line. You can also change the `ListenPort` line if you would like WireGuard to be available on a different port:

```
# /etc/wireguard/wg0.conf
[Interface]
PrivateKey = base64_encoded_private_key_goes_here
Address = 10.8.0.1/24
ListenPort = 51820
# SaveConfig = true
```

The `SaveConfig` line ensures that when a WireGuard interface is shutdown, any changes will get saved to the configuration file.

**When using multi peers, disable SaveConfig in case when some peer is not connected to the tunnel, wg deletes its corresponding public key.**

Save and close the `/etc/wireguard/wg0.conf` file.
  
## Step 4 — Adjusting the WireGuard Server’s Network Configuration

If you are using WireGuard to connect a peer to the WireGuard Server in order to access services on the **server only**, then you do not need to complete this section. If you would like to route your WireGuard Peer’s Internet traffic through the WireGuard 
Server then you will need to configure IP forwarding by following this section of the tutorial.

To configure forwarding, open the `/etc/sysctl.conf` file using `vim` or your preferred editor:

```shell
sudo vim /etc/sysctl.conf
```

If you are using IPv4 with WireGuard, add the following line at the bottom of the file:

/etc/sysctl.conf

```
net.ipv4.ip_forward=1
```

If you are using IPv6 with WireGuard, add this line at the bottom of the file:

/etc/sysctl.conf

```
net.ipv6.conf.all.forwarding=1
```

If you are using both IPv4 and IPv6, ensure that you include both lines. Save and close the file when you are finished.

To read the file and load the new values for your current terminal session, run:

```shell
sudo sysctl -p
```

```
Outputnet.ipv6.conf.all.forwarding = 1
net.ipv4.ip_forward = 1
```

Now your WireGuard Server will be able to forward incoming traffic from the virtual VPN ethernet device to others on the server, and from there to the public Internet. Using this configuration will allow you to route all web traffic from your WireGuard Peer via your server’s IP address, and your client’s public IP address will be effectively hidden.

However, before traffic can be routed via your server correctly, you will need to configure some firewall rules. These rules will ensure that traffic to and from your WireGuard Server and Peers flows properly.
  
## Step 5 — Configuring the WireGuard Server’s Firewall

In this section you will edit the WireGuard Server’s configuration to add firewall rules that will ensure traffic to and from the server and clients is routed correctly. As with the previous section, skip this step if you are only using your WireGuard VPN for a machine to machine connection to access resources that are restricted to your VPN.

To allow WireGuard VPN traffic through the Server’s firewall, you’ll need to enable masquerading, which is an iptables concept that provides on-the-fly dynamic network address translation (NAT) to correctly route client connections.

First find the public network interface of your WireGuard Server using the `ip route` sub-command:

```shell
ip route list default
```

The public interface is the string found within this command’s output that follows the word “dev”. For example, this result shows the interface named `eth0`, which is highlighted below:

```
default via 107.173.xx.yy dev eth0 onlink
```

Note your device’s name since you will add it to the `iptables` rules in the next step.

To add firewall rules to your WireGuard Server, open the `/etc/wireguard/wg0.conf` file with `vim` or your preferred editor again.

```shell
sudo vim /etc/wireguard/wg0.conf
```

**The configuration of DigitalOcean doesn't work for me, this is the modified version that works:**

```
PostUp = ufw route allow in on wg0 out on eth0
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PreDown = ufw route delete allow in on wg0 out on eth0
PreDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```

The `PostUp` lines will run when the WireGuard Server starts the virtual VPN tunnel. In the example here, it will add three `ufw` and `iptables` rules:
  
`ufw route allow in on wg0 out on eth0` - This rule will allow forwarding IPv4 and IPv6 traffic that comes in on the `wg0` VPN interface to the `eth0` network interface on the server. It works in conjunction with the `net.ipv4.ip_forward` and `net.ipv6.conf.all.forwarding` sysctl values that you configured in the previous section.
  
`iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE` - This rule configures masquerading, and rewrites IPv4 traffic that comes in on the `wg0` VPN interface to make it appear like it originates directly from the WireGuard Server’s public IPv4 address.
  
`ip6tables -t nat -I POSTROUTING -o eth0 -j MASQUERADE` - This rule configures masquerading, and rewrites IPv6 traffic that comes in on the `wg0` VPN interface to make it appear like it originates directly from the WireGuard Server’s public IPv6 address.

The `PreDown` rules run when the WireGuard Server stops the virtual VPN tunnel. These rules are the inverse of the `PostUp` rules, and function to undo the forwarding and masquerading rules for the VPN interface when the VPN is stopped.

In both cases, edit the configuration to include or exclude the IPv4 and IPv6 rules that are appropriate for your VPN. For example, if you are just using IPv4, then you can exclude the lines with the `ip6tables` commands.

Conversely, if you are only using IPv6, then edit the configuration to only include the `ip6tables` commands. The `ufw` lines should exist for any combination of IPv4 and IPv6 networks. Save and close the file when you are finished.

The last part of configuring the firewall on your WireGuard Server is to allow traffic to and from the WireGuard UDP port itself. If you did not change the port in the server’s `/etc/wireguard/wg0.conf` file, the port that you will open is `51820`. If you chose a different port when editing the configuration be sure to substitute it in the following UFW command.

In case you forgot to open the SSH port when following the prerequisite tutorial, add it here too:

```shell
sudo ufw allow 51820/udp
sudo ufw allow OpenSSH
```

**Note**: If you are using a different firewall or have customized your UFW configuration, you may need to add additional firewall rules. For example, if you decide to tunnel all of your network traffic over the VPN connection, you will need to ensure that port `53` traffic is allowed for DNS requests, and ports like `80` and `443` for HTTP and HTTPS traffic respectively. If there are other protocols that you are using over the VPN then you will need to add rules for them as well.

After adding those rules, disable and re-enable UFW to restart it and load the changes from all of the files you’ve modified:

```shell
sudo ufw disable
sudo ufw enable
```
  
## Step 6 — Starting the WireGuard Server

WireGuard can be configured to run as a `systemd` service using its built-in `wg-quick` script. While you could manually use the `wg` command to create the tunnel every time you want to use the VPN, doing so is a manual process that becomes repetitive and error prone. Instead, you can use `systemctl` to manage the tunnel with the help of the `wg-quick` script.

Using a `systemd` service means that you can configure WireGuard to start up at boot so that you can connect to your VPN at any time as long as the server is running. To do this, enable the `wg-quick` service for the `wg0` tunnel that you’ve defined by adding it to `systemctl`:

```shell
sudo systemctl enable wg-quick@wg0.service
```

Notice that the command specifies the name of the tunnel `wg0` device name as a part of the service name. This name maps to the `/etc/wireguard/wg0.conf` configuration file. This approach to naming means that you can create as many separate VPN tunnels as you would like using your server.

For example, you could have a tunnel device and name of `prod` and its configuration file would be `/etc/wireguard/prod.conf`. Each tunnel configuration can contain different IPv4, IPv6, and client firewall settings. In this way you can support multiple different peer connections, each with their own unique IP addresses and routing rules.

Now start the service:

```shell
sudo systemctl start wg-quick@wg0.service
```

Double check that the WireGuard service is active with the following command. You should see `active (running)` in the output:

```shell
sudo systemctl status wg-quick@wg0.service
```
  
## Step 7 — Configuring a WireGuard Peer

Configuring a WireGuard peer is similar to setting up the WireGuard 
Server. Once you have the client software installed, you’ll generate a 
public and private key pair, decide on an IP address or addresses for 
the peer, define a configuration file for the peer, and then start the 
tunnel using the `wg-quick` script.

You can add as many peers as you like to your VPN by generating a key
pair and configuration using the following steps. If you add multiple 
peers to the VPN be sure to keep track of their private IP addresses to 
prevent collisions.

To configure the WireGuard Peer, ensure that you have the WireGuard package installed using the following `apt` commands. On the WireGuard peer run:

```shell
sudo apt update
sudo apt install wireguard
```
  
### Creating the WireGuard Peer’s Key Pair

Next, you’ll need to generate the key pair on the peer using the same steps as you used on the server. From your local machine or remote server that will serve as peer, proceed and create the private key for the peer using the following commands:

```shell
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
```

Again you will receive a single line of `base64` encoded output, which is the private key. A copy of the output is also stored in the `/etc/wireguard/private.key`. Carefully make a note of the private key that is output since you’ll need to add it to WireGuard’s configuration file later in this section.

Next use the following command to create the public key file:

```shell
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

You will again receive a single line of `base64` encoded output, which is the public key for your WireGuard Peer. Copy it somewhere for reference, since you will need to distribute the public key to the WireGuard Server in order to establish an encrypted 
connection.
  
### Creating the WireGuard Peer’s Configuration File

Now that you have a key pair, you can create a configuration file for
the peer that contains all the information that it needs to establish a
connection to the WireGuard Server.

You will need a few pieces of information for the configuration file:
  
The `base64` encoded private key that you generated on the peer.
  
The IPv4 and IPv6 address ranges that you defined on the WireGuard Server.
  
The `base64` encoded public key from the WireGuard Server.
  
The public IP address and port number of the WireGuard Server. 
Usually this will be the IPv4 address, but if your server has an IPv6 
address and your client machine has an IPv6 connection to the internet 
you can use this instead of IPv4.

With all this information at hand, open a new `/etc/wireguard/wg0.conf` file on the WireGuard Peer machine using `vim` or your preferred editor:

```shell
sudo vim /etc/wireguard/wg0.conf
```

Add the following lines to the file, substituting in the various data into the highlighted sections as required:

```
[Interface]
PrivateKey = base64_encoded_peer_private_key_goes_here
Address = 10.8.0.2/24

[Peer]
PublicKey = U9uE2kb/nrrzsEU58GD3pKFU3TLYDMCbetIsnV8eeFE=
AllowedIPs = 10.8.0.0/24
Endpoint = 203.0.113.1:51820
```

Notice how the first `Address` line uses an IPv4 address from the `10.8.0.0/24` subnet that you chose earlier. This IP address can be anything in the subnet as long as it is different from the server’s IP. Incrementing addresses by 1 each time you add a peer is generally the easiest way to allocate IPs.

Likewise, notice how the second `Address` line uses an IPv6 address from the subnet that you generated earlier, and increments the server’s address by one. Again, any IP in the range is valid if you decide to use a different address.

The other notable part of the file is the last `AllowedIPs` line. These two IPv4 and IPv6 ranges instruct the peer to only send traffic over the VPN if the destination system has an IP address in either range. Using the `AllowedIPs` directive, you can restrict the VPN on the peer to only connect to other peers and services on the VPN, or you can configure the setting to tunnel all traffic over the VPN and use the WireGuard Server as a gateway.

If you are only using IPv4, then omit the trailing `fd0d:86fa:c3bc::/64` range (including the `,` comma). Conversely, if you are only using IPv6, then only include the `fd0d:86fa:c3bc::/64` prefix and leave out the `10.8.0.0/24` IPv4 range.

In both cases, if you would like to send all your peer’s traffic over the VPN and use the WireGuard Server as a gateway for all traffic, then you can use `0.0.0.0/0`, which represents the entire IPv4 address space, and `::/0` for the entire IPv6 address space.
  
### (Optional) Configuring a Peer to Route All Traffic Over the Tunnel

**I did not configure this but it still works.**

If you have opted to route all of the peer’s traffic over the tunnel using the `0.0.0.0/0` or `::/0` routes and the peer is a remote system, then you will need to complete 
the steps in this section. If your peer is a local system then it is 
best to skip this section.

For remote peers that you access via SSH or some other protocol using
a public IP address, you will need to add some extra rules to the 
peer’s `wg0.conf` file. These rules will ensure that you can 
still connect to the system from outside of the tunnel when it is 
connected. Otherwise, when the tunnel is established, all traffic that 
would normally be handled on the public network interface will not be 
routed correctly to bypass the `wg0` tunnel interface, leading to an inaccessible remote system.

First, you’ll need to determine the IP address that the system uses as its default gateway. Run the following `ip route` command:

```
ip route list table main default
```

You will receive output like the following:

```
default via 203.0.113.1 dev eth0 proto static
```

Note the gateway’s highlighted IP address `203.0.113.1` for later use, and device `eth0`. Your device name may be different. If so, substitute it in place of `eth0` in the following commands.

Next find the public IP for the system by examining the device with the `ip address show` command:

```
ip -brief address show eth0
```

You will receive output like the following:

```
Outputeth0             UP             203.0.113.5/20 10.20.30.40/16 2604:a880:400:d1::3d3:6001/64 fe80::68d5:beff:feff:974c/64
```

In this example output, the highlighted `203.0.113.5` IP (without the trailing `/20`) is the public address that is assigned to the `eth0` device that you’ll need to add to the WireGuard configuration.

Now open the WireGuard Peer’s `/etc/wireguard/wg0.conf` file with `nano` or your preferred editor.

```
sudo nano /etc/wireguard/wg0.conf
```

Before the `[Peer]` line, add the following 4 lines:

```
PostUp = ip rule add table 200 from 203.0.113.5
PostUp = ip route add table 200 default via 203.0.113.1
PreDown = ip rule delete table 200 from 203.0.113.5
PreDown = ip route delete table 200 default via 203.0.113.1

[Peer]
. . .
```

These lines will create a custom routing rule, and add a custom route
to ensure that public traffic to the system uses the default gateway.
  
`PostUp = ip rule add table 200 from 203.0.113.5` - This command creates a rule that checks for any routing entries in the table numbered `200` when the IP matches the system’s public `203.0.113.5` address.
  
`PostUp = ip route add table 200 default via 203.0.113.1` - This command ensures that any traffic being processed by the `200` table will use the `203.0.113.1` gateway for routing, instead of the WireGuard interface.

The `PreDown` lines remove the custom rule and route when the tunnel is shutdown.

Note: The table number `200` is arbitrary when constructing these rules. You can use a value between 2 and 252, or you can use a custom name by adding a label to the `/etc/iproute2/rt_tables` file and then referring to the name instead of the numeric value.

For more information about how routing tables work in Linux visit the [Routing Tables Section](http://linux-ip.net/html/routing-tables.html) of the [Guide to IP Layer Network Administration with Linux](http://linux-ip.net/html/index.html).

If you are routing all the peer’s traffic over the VPN, ensure that you have configured the correct `sysctl` and `iptables` rules on the WireGuard Server in [Step 4 — Adjusting the WireGuard Server’s Network Configuration](https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04#step-4-%E2%80%94-adjusting-the-wireguard-server-39-s-network-configuration) and [Step 5 — Configuring the WireGuard Server’s Firewall](https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04#step-5-%E2%80%94-configuring-the-wireguard-server%E2%80%99s-firewall).
  
### (Optional) Configuring the WireGuard Peer’s DNS Resolvers

**I use OpenDNS(208.67.222.222) as DNS server because resolvectl dns didn't give a valid output.**

If you are using the WireGuard Server as a VPN gateway for all your peer’s traffic, you will need to add a line to the `[Interface]` section that specifies DNS resolvers. If you do not add this setting, 
then your DNS requests may not be secured by the VPN, or they might be 
revealed to your Internet Service Provider or other third parties.

If you are only using WireGuard to access resources on the VPN 
network or in a peer-to-peer configuration then you can skip this 
section.

To add DNS resolvers to your peer’s configuration, first determine 
which DNS servers your WireGuard Server is using. Run the following 
command on the **WireGuard Server**, substituting in your ethernet device name in place of `eth0` if it is different from this example:

```
resolvectl dns eth0
```

You should receive output like the following:

```
OutputLink 2 (eth0): 67.207.67.2 67.207.67.3 2001:4860:4860::8844 2001:4860:4860::8888
```

The IP addresses that are output are the DNS resolvers that the 
server is using. You can choose to use any or all of them, or only IPv4 
or IPv6 depending on your needs. Make a note of the resolvers that you 
will use.

Next you will need to add your chosen resolvers to the WireGuard Peer’s configuration file. Back on the **WireGuard Peer**, open `/etc/wireguard/wg0.conf` file using `nano` or your preferred editor:

```
sudo nano /etc/wireguard/wg0.conf
```

Before the `[Peer]` line, add the following:

```
DNS = 67.207.67.2 2001:4860:4860::8844

[Peer]
. . .
```

Again, depending on your preference or requirements for IPv4 and IPv6, you can edit the list according to your needs.

Once you are connected to the VPN in the following step, you can 
check that you are sending DNS queries over the VPN by using a site like
[DNS leak test.com](https://www.dnsleaktest.com).

You can also check that your peer is using the configured resolvers with the `resolvectl dns` command like you ran on the server. You should receive output like the 
following, showing the DNS resolvers that you configured for the VPN 
tunnel:

```
OutputGlobal: 67.207.67.2 67.207.67.3
. . .
```

With all of these DNS resolver settings in place, you are now ready to add the peer’s public key to the server, and then start the WireGuard tunnel on the peer.
  
## Step 8 — Adding the Peer’s Public Key to the WireGuard Server

```shell
sudo wg set wg0 peer <client publickey> allowed-ips 10.5.0.2,fd0d:86fa:c3bc::2
```

This is OK but I think it's better to modify the server.conf.
 