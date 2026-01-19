# Ubuntu Networking Lab – SSH Connectivity Issue

## 1. Scenario (Problem Statement)
User cannot SSH into Ubuntu virtual machine hosted in Azure.

---

## 2. Investigation Steps (High Level)
- Verified that the SSH service was installed and running
- Checked whether the SSH daemon was listening on the expected port
- Inspected local firewall configuration on the VM
- Reviewed Azure Network Security Group (NSG) inbound rules
- Analyzed packet flow to determine where traffic was being blocked

---

## 3. Key Commands Used
- `ss -tuln`  
  Verified whether port 22 was actively listening on the system.
- `systemctl status ssh`  
  Confirmed SSH service state and startup behavior.
- `journalctl -u ssh`  
  Reviewed SSH service logs for errors or denied connections.
- `ip a`  
  Verified network interface configuration and IP assignment.

---

## 4. Root Cause
Inbound SSH traffic was blocked by Azure Network Security Group rules.

---

## 5. Resolution
Updated the NSG to allow inbound TCP traffic on port 22 from a trusted source IP.

---

## 6. Verification
Successfully established an SSH connection to the Ubuntu VM after NSG update.
