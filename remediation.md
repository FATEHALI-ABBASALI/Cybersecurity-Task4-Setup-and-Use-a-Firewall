🔧 Task 4 - Remediation Notes 🔧

1.  🖥️  Summary  
    . Purpose : Reduce attack surface by blocking insecure services (Telnet) and allowing management access (SSH).  
    . Scope : Local machine firewall rules on Windows and Linux.

2.  🔒  Actions taken (Windows)  
    . Blocked Telnet (TCP 23) to prevent plaintext remote logins.  
    . Command used : New-NetFirewallRule -DisplayName "Block Telnet Port 23 (Task4-Test)" -Direction Inbound -LocalPort 23 -Protocol TCP -Action Block -Profile Any  
    . Kept SSH (TCP 22) allowed for remote management.  
    . Exported firewall rules for backup : netsh advfirewall export "C:\Task4-Firewall\firewall-backup.wfw"

3.  🔐  Actions taken (Linux)  
    . Default policy set to deny incoming and allow outgoing.  
    . Blocked Telnet (TCP 23) : sudo ufw deny 23/tcp comment 'Block telnet - Task4 test'  
    . Allowed SSH (TCP 22) : sudo ufw allow 22/tcp comment 'Allow SSH'  
    . Saved rules output : sudo ufw status numbered > ~/Task4-Firewall/ufw-status.txt

4.  📌  Why these changes?  
    . Telnet is insecure (plaintext) and exposes credentials. Blocking reduces risk.  
    . SSH is required for secure remote management; keep it restricted and use key-based auth where possible.  
    . Regularly update and patch services to reduce vulnerability risk.

5.  ✅  Next steps / recommendations  
    . Apply system updates immediately when critical patches are available.  
    . Use strong passwords and SSH key authentication; disable password auth for SSH if possible.  
    . Regularly review firewall rules and maintain backups of rule exports.  
    . Monitor firewall logs for suspicious connection attempts.
