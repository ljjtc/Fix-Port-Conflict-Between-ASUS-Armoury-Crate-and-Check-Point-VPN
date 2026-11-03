Fix Port Conflict Between ASUS Armoury Crate and Check Point VPN

Problem: Check Point Mobile VPN E89.20 shows:

VPN Service is down.

Cause: ASUS Armoury Crate (ArmouryCrateControlInterface) occupies TCP port 7778, which conflicts with Check Point VPN.

Solution: Change the Check Point service port from 7778 to 17778.

Open PowerShell as Administrator and run:

Stop-Service TracSrvWrapper
Set-ItemProperty `
  -Path "HKLM:\SOFTWARE\WOW6432Node\CheckPoint\TRAC" `
  -Name "trac_port" -Value 17778
Start-Service TracSrvWrapper

Verify:

Get-ItemPropertyValue `
  "HKLM:\SOFTWARE\WOW6432Node\CheckPoint\TRAC" `
  -Name "trac_port"

Expected output: 17778.

The Check Point VPN client should now work normally.

Note: A Check Point update or repair may reset the port to 7778.