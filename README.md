<h1>🔥 Task 4 - Firewall Setup and Testing 🔥</h1>

<h3>🖥️ Environment Details</h3>
<p>Operating System: Windows 10 Pro and Ubuntu 22.04 LTS</p>
<p>Tools Used: Windows Defender Firewall (GUI + PowerShell + netsh) and UFW (Linux)</p>
<p>Deliverables: Firewall config exports, screenshots, README.md, remediation.md</p>

<h3>⚙️ Steps on Windows</h3>
<pre>
Open Windows Defender Firewall with Advanced Security (wf.msc)

List current rules (PowerShell):
Get-NetFirewallRule -Direction Inbound | Select-Object DisplayName, Enabled, Action, Direction

List current rules (netsh):
netsh advfirewall firewall show rule name=all

Create inbound rule to block Telnet port 23 (GUI):
Inbound Rules → New Rule → Port → TCP 23 → Block → Save as "Block Telnet Port 23 (Task4-Test)"

Create inbound rule to block Telnet port 23 (PowerShell):
New-NetFirewallRule -DisplayName "Block Telnet Port 23 (Task4-Test)" -Direction Inbound -LocalPort 23 -Protocol TCP -Action Block -Profile Any

Create inbound rule to block Telnet port 23 (netsh):
netsh advfirewall firewall add rule name="Block Telnet Port 23 (Task4-Test)" dir=in action=block protocol=TCP localport=23

Test the rule:
Test-NetConnection -ComputerName 127.0.0.1 -Port 23
Expected: TcpTestSucceeded : False

Allow SSH if needed:
New-NetFirewallRule -DisplayName "Allow SSH (22)" -Direction Inbound -LocalPort 22 -Protocol TCP -Action Allow -Profile Any

Export firewall policy backup:
netsh advfirewall export "C:\Task4-Firewall\firewall-backup.wfw"

Delete the test Telnet rule:
netsh advfirewall firewall delete rule name="Block Telnet Port 23 (Task4-Test)" protocol=TCP localport=23
</pre>

<h3>🐧 Steps on Linux (UFW)</h3>
<pre>
Install and enable UFW:
sudo apt update
sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable

Check status:
sudo ufw status numbered

Block inbound port 23 (Telnet):
sudo ufw deny 23/tcp comment 'Block telnet - Task4 test'

Test the rule:
nc -vz localhost 23
or from another machine:
nmap -p 23 &lt;linux-ip&gt;

Allow SSH port 22:
sudo ufw allow 22/tcp comment 'Allow SSH'

Delete the Telnet block rule:
sudo ufw delete deny 23/tcp

Save current rules output:
sudo ufw status numbered &gt; ~/Task4-Firewall/ufw-status.txt
</pre>

<h3>📸 Screenshots Required</h3>
<p>windows-inbound-rule-block-telnet.png — Windows inbound rules list after adding Telnet block</p>

<p>windows-test-netconnection-port23.png — Windows PowerShell Test-NetConnection result</p>
<p>windows-export-policy.png — Windows firewall export file confirmation</p>
<p>linux-ufw-status-block-telnet.png — Linux UFW status numbered after deny rule</p>
<p>linux-test-port23.png — Linux nc or nmap test result</p>

