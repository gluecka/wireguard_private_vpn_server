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
<span><em>directory: -->   </em></span><span>/etc/default</span><br>
<span><em>run: -->   </em></span><span>vim keyboard</span><br>
<span><em>modify and save configfile: -->   </em></span><span>XKLAYOUT: 'us' --> 'de'</span><br>
<span><em>run: -->   </em></span><span>reboot</span><br>

<h4>Set network of the VM on bridge mode</h4>
<p>Go in VM window to menu "machine" --> "change" --> "network" --> "bridge mode"<br>
RESTART VM machine</p>


<h4>Activate SSH Access</h4>
<span><em>run: -->   </em></span><span>ip route</span><br>
<p>Get information of interface (et0...) and the private IP of the Linux server or from the Admin interface of the router.</p><br>
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


<h3>Wireguard Configuration IP-Forwarding</h3>


<h4>Activate IP-Forwarding</h4>

<h3>Wireguard Server Configuration wg0.conf</h3>


<h3>Wireguard Keygen</h3>

<h4>Activate Wireguard</h4>

<h3>Firewall Configuration UFW</h3>


<h3>Peering and Port Forwarding</h3>


<h3>QR-Code Peering</h3>