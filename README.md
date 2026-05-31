<h1>Wireguard Private VPN-Server</h1>
<p><b>This is the description to build and configure a private Wireguard VPN Server running on a virtuell Linux machine provided of VMBox.</b></p>

<h3>Install the provider of the virtuell machine</h3>
<a href=https://www.oracle.com/de/virtualization/technologies/vm/downloads/virtualbox-downloads.html>Oracle VirtualBox Download Page</a><br>

<h3>Deploy the virtuell Linux Server LTS</h3>
<a href=https://ubuntu.com/download/server>Linux Ubuntu 26.04. liver server LTS .iso Download Page</a><br>

<p>Create a new virtuell machine with the Linux .iso on VMBox.<br>
Set the installation configuration EFI and guest on YES.<br>
Put follow ressource into VM:<br>
2 Core<br>
2 GB RAM (min)
</p>

<h4>Keyboard Configuration</h4>
<p>Tip --> Ctl-ö == :</p>
<p>Tip --> ? == -</p>
<span><em>directory: -->   </em></span><span>/etc/default</span><br>
<span><em>run: -->   </em></span><span>vim keyboard</span><br>
<span><em>modify and save configfile: -->   </em></span><span>XKLAYOUT: 'us' --> 'de'</span><br>
<span><em>run: -->   </em></span><span>reboot</span><br>

<h4>Set network of the VM on bridge mode</h4>
<p>Go in VM window to menu "machine" --> "change" --> "network" --> "bridge mode"<br>
RESTART VM machine</p>


<h4>Activate SSH Access</h4>
<span><em>run: -->   </em></span><span>ip route</span><br>
<p>Get information of the private IP of the Linux server in command line. Or get the same information from the Admin interface of the router.</p><br>
<span><em>run: -->   </em></span><span>apt-get update</span><br>
<span><em>run: -->   </em></span><span>apt-get upgrade -y</span><br>
<span><em>run: -->   </em></span><span>apt-get install openssh-server</span><br>
<span><em>run: -->   </em></span><span>systemctl start ssh</span><br>
<span><em>run: -->   </em></span><span>systemctl enable ssh</span><br>
<span><em>run: -->   </em></span><span>systemctl status ssh</span><br>
<span><em>run: -->   </em></span><span>reboot</span><br>
<p>Test the SSH connection with an other device in same network as the VM:</p>
<span><em>run: -->   </em></span><span>ssh USER_VM@PRIVATE_IP</span><br>
<h3></h3>


<h3>Install Wireguard</h3>
<span><em>run: -->   </em></span><span>apt-get install wireguard</span><br>

<h3>Wireguard Configuration IP-Forwarding</h3>
<span><em>directory: -->   </em></span><span>/etc/sysctl.d</span><br>
<p>Copy the file '99-wireguard.conf' from the repository in the defined directory</p>
<span><em>run: -->   </em></span><span>chmod 600 99-wireguard.conf</span><br>

<h4>Activate IP-Forwarding</h4>
<span><em>run: -->   </em></span><span>sysctl --system</span><br>
<p>wireguard.conf is activate</p>

<h3>Wireguard Server Configuration wg0.conf</h3>
<span><em>directory: -->   </em></span><span>/etc/wireguard/</span><br>
<p>Copy the file 'wg0.conf' from the repository in the defined directory</p>

<h4>Wireguard Keygen</h4>
<span><em>directory: -->   </em></span><span>/etc/wireguard/</span><br>
<span><em>run: -->   </em></span><span>wg genkey | tee client_private.key | wg pubkey > client_public.key</span><br>
<span><em>run: -->   </em></span><span>chmod 600 client_private.key</span><br>
<span><em>run: -->   </em></span><span>chmod 600 client_public.key</span><br>
<span><em>run: -->   </em></span><span>cat client_private.key</span><br>
<p>Copy private.key and place that in wg0.conf as parameter: 'PrivateKey'</p>

<h4>Network interface</h4>
<span><em>run: -->   </em></span><span>ip a "or" ip route</span><br>
<p>Check wich interface the network use (enp0s3, eth0, ....)</p>
<p>Replace the correct interface in wg0.conf in the two lines of 'POSTROUTING'</p>

<h4>Activate Wireguard</h4>
<span><em>run: -->   </em></span><span>systemctl start wg-quick@wg0</span><br>
<span><em>run: -->   </em></span><span>systemctl enable wg-quick@wg0</span><br>
<span><em>run: -->   </em></span><span>systemctl status wg-quick@wg0</span><br>

<h3>Firewall Configuration UFW</h3>
<span><em>run: -->   </em></span><span>systemctl start ufw</span><br>
<span><em>run: -->   </em></span><span>systemctl enable ufw</span><br>
<span><em>run: -->   </em></span><span>systemctl status ufw</span><br><br>

<span><em>run: -->   </em></span><span>ufw enable</span><br>
<span><em>run: -->   </em></span><span>ufw status</span><br><br>

<span><em>run: -->   </em></span><span>ufw allow 51820/udp</span><br>
<span><em>run: -->   </em></span><span>ufw allow 22/tcp</span><br><br>

<span><em>run: -->   </em></span><span>ufw status</span><br>
<span><em>run: -->   </em></span><span>ufw status | grep 51820</span><br>


<h3>Peering and Port Forwarding</h3>
<a href=https://www.wireguard.com/install>Wireguard Client Download Page</a><br>
<p>Download Client APP for Smartphones in APP-Store</p>
<p>1. Add empty tunnel</p>
<p>2. Copy the public.key of vpn-server and place that in Client config</p>
<p>3. Copy the public.key of vpn-client and place that in wg0.config</p>
<p>4. Recommended all Line´s of Peer Client 1 with one # in wg0.conf</p>
<p>5. Check the Allowed IP´s. It have to be the same as in the [Interface] of the Client. Choose an IP in Client in range of the vpn-network!</p>
<p>6. Private.key of Client is generated automaticly.</p><br>
<p>Details see in the file 'example_client.conf'</p>


<span><em>run: -->   </em></span><span>systemctl restart wg-quick@wg0</span><br>

<p>Activate the Client!</p><br>
<span><em>run: -->   </em></span><span>wg</span><br>
<p>If the peering with the public.key have an info "latest handshake..." the connection is running.</p><br>

<h3>QR-Code Peering</h3>
<p>Peer a Client easy with an QR-Code:</p>
<span><em>run: -->   </em></span><span>apt-get install quencode</span><br>
<span><em>directory: -->   </em></span>/etc/wireguard/<span></span><br><br>
<p>Show QR-Code in shell</p>
<span><em>run: -->   </em></span><span>qrencode -t ansiutf8 < /etc/wireguard/wg0.conf</span><br><br>
<p>Safe QR-Code in a png fiel for sharing</p>
<span><em>run: -->   </em></span><span>qrencode -t png -o client_qr.png < /etc/wireguard/wg0.conf</span><br>


<h3>Different commands to finde issues</h3>
<p>Check listen port 51820</p>
<span><em>run: -->   </em></span><span>ss -ulnp | grep 51820</span><br><br>
<p>Show Wireguard Logs</p>
<span><em>run: -->   </em></span><span>journalctl -xeu wg-quick@wg0</span><br><br>
<p>Show Wireguard Logs for wg0 only. With -eu see the latest logs only.</p>
<span><em>run: -->   </em></span><span>journalctl -u wg-quick@wg0</span><br><br>
<p>Check interface, masquerade, ...</p>
<span><em>run: -->   </em></span><span>iptables -t nat -L -n -v</span><br>