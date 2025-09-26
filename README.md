🔥 Task 4 - Firewall Setup and Testing 🔥

1.  🖥️  Environment Details  
    . Operating System : Windows 10 Pro and Ubuntu 22.04 LTS  
    . Tools Used : Windows Defender Firewall (GUI + PowerShell + netsh) and UFW (Linux)  
    . Deliverables : Firewall config exports, screenshots, README.md, remediation.md  

2.  ⚙️  Steps on Windows  
    . Open Windows Defender Firewall with Advanced Security (wf.msc)  
    . List current rules :  
        Command PowerShell :  
        Get-NetFirewallRule -Direction Inbound | Select-Object DisplayName, Enabled, Action, Direction  
        Command netsh :  
        netsh advfirewall firewall show rule name=all  
    . Create inbound rule to block Telnet port 23 :  
        GUI : Inbound Rules → New Rule → Port 23 → Block → Save as "Block Telnet Port 23 (Task4-Test)"  
        Command PowerShell :  
        New-NetFirewallRule -DisplayName "Block Telnet Port 23 (Task4-Test)" -Direction Inbound -LocalPort 23 -Protocol TCP -Action Block -Profile Any  
        Command netsh :  
        netsh advfirewall firewall add rule name="Block Telnet Port 23 (Task4-Test)" dir=in action=block protocol=TCP localport=23  
    . Test the rule :  
        Test-NetConnection -ComputerName 127.0.0.1 -Port 23  
        Expected Output : TcpTestSucceeded : False  
    . Allow SSH port 22 (if needed) :  
        New-NetFirewallRule -DisplayName "Allow SSH (22)" -Direction Inbound -LocalPort 22 -Protocol TCP -Action Allow -Profile Any  
    . Export firewall policy backup :  
        netsh advfirewall export "C:\Task4-Firewall\firewall-backup.wfw"  
    . Delete the test Telnet rule :  
        netsh advfirewall firewall delete rule name="Block Telnet Port 23 (Task4-Test)" protocol=TCP localport=23  

3.  🐧  Steps on Linux (UFW)  
    . Install and enable UFW :  
        sudo apt update  
        sudo apt install ufw -y  
        sudo ufw default deny incoming  
        sudo ufw default allow outgoing  
        sudo ufw enable  
    . Check status :  
        sudo ufw status numbered  
    . Block inbound port 23 (Telnet) :  
        sudo ufw deny 23/tcp comment 'Block telnet - Task4 test'  
    . Test the rule :  
        nc -vz localhost 23  
        or from another machine :  
        nmap -p 23 <linux-ip>  
    . Allow SSH port 22 :  
        sudo ufw allow 22/tcp comment 'Allow SSH'  
    . Delete the Telnet block rule :  
        sudo ufw delete deny 23/tcp  
    . Save current rules output :  
        sudo ufw status numbered > ~/Task4-Firewall/ufw-status.txt  

4.  📸  Screenshots Required  
    . Windows inbound rules list after adding Telnet block → save as windows-inbound-rule-block-telnet.png  
    . Windows PowerShell Test-NetConnection result → save as windows-test-netconnection-port23.png  
    . Windows firewall export file confirmation → save as windows-export-policy.png  
    . Linux UFW status numbered after deny rule → save as linux-ufw-status-block-telnet.png  
    . Linux nc or nmap test result → save as linux-test-port23.png  

5.  📂  Repository Structure  
    . README.md (this file)  
    . remediation.md (explain why ports were blocked or allowed)  
    . firewall-backup.wfw (Windows export file)  
    . ufw-status.txt (Linux rules output file)  
    . screenshots/  
        windows-inbound-rule-block-telnet.png  
        windows-test-netconnection-port23.png  
        windows-export-policy.png  
        linux-ufw-status-block-telnet.png  
        linux-test-port23.png  

6.  ✅  Verification  
    . Verify Telnet port 23 is blocked → Test-NetConnection / nmap should fail  
    . Verify SSH port 22 is allowed → connection should succeed  
    . Verify exported config and rules file exist in repo
